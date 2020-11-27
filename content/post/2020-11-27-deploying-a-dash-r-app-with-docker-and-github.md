---
layout: post
title: Deploying a Dash R App with Docker and GitHub
date: 2020-11-26T21:59:16.606Z
external-url: ""
---
If you've stumbled upon this, you're probably trying to find the answer to how to serve a Dash R app over the web. I know that I originally had a hard time when I was working on a [COVID-related app](https://covid.justinsingh.me)... You can also look at [Plotly's tutorial on deploying Dash apps to Heroku](https://dashr.plotly.com/deployment), which is helpful if you don't want to manage your own environment! 

**Note: if your app requires a lot of memory (i.e. a GIS application), deploying to Heroku may not be possible without proper memory management.**

However, maybe you're just interested in Dash and want to consider whether to use Dash or Shiny. *I'll say, I'm much more in favor of Dash after using both, but I'll let you make that decision.*

In this post, I'll run through the general concept of deploying a Dash app hosted in a GitHub repo (or any git repo in fact), and built in a Docker container. Despite the use of Docker, this post will be useful if you want to deploy it locally or outside of a container, say on a bare VPS.

## Some Context

### What is [Dash](https://dashr.plotly.com/introduction)?

> `Dash` is a productive framework for building web applications in both `R` and `Python`.
>
> Written on top of [`Fiery`](https://github.com/thomasp85/fiery)/[`Flask`](https://flask.palletsprojects.com/en/1.1.x/), [`Plotly.js`](https://plotly.com/javascript/), and [`React.js`](https://reactjs.org/), Dash is ideal for building data visualization apps with highly custom user interfaces in pure `R` or `Python`. It's particularly suited for anyone who works with data.
>
> ![](/images/thumbnail_0a718df0-9ce7-11e9-8982-0242ac11004a.png)

*Source: [Introduction to Dash](https://dashr.plotly.com/introduction)*

### What is [Docker](https://www.docker.com/)?

> Docker container technology was launched in 2013 as an open source [Docker Engine](https://www.docker.com/products/container-runtime).
>
> It leveraged existing computing concepts around containers and specifically in the Linux world, primitives known as cgroups and namespaces. Docker's technology is unique because it focuses on the requirements of developers and systems operators to separate application dependencies from infrastructure.

#### Containers vs. Virtual Machines

> **CONTAINERS**
>
> Containers are an abstraction at the app layer that packages code and dependencies together. Multiple containers can run on the same machine and share the OS kernel with other containers, each running as isolated processes in user space. Containers take up less space than VMs (container images are typically tens of MBs in size), can handle more applications and require fewer VMs and Operating systems.
>
> **VIRTUAL MACHINES**
>
> Virtual machines (VMs) are an abstraction of physical hardware turning one server into many servers. The hypervisor allows multiple VMs to run on a single machine. Each VM includes a full copy of an operating system, the application, necessary binaries and libraries - taking up tens of GBs. VMs can also be slow to boot.
>
> ![](/images/container-vm-whatcontainer_2.png)
>
> ![](/images/docker-containerized-appliction-blue-border_2.png)

*Source: [Docker](https://www.docker.com/resources/what-container)*

## Deployment Process

Now that we have some context to the tools we will be using, let's take a deeper look at the process and how Dash handles serving applications.

### Dash App Example

For this post, I'll be using my [COVID Data App](https://covid.justinsingh.me) as an example. If you take a look at the [GitHub repo](https://github.com/program--/covid-tracker), you'll see a couple of *key* files:

- `Dockerfile`: Docker build instructions
- `app.R`: Dash app script
- `renv.lock`: Dependency management with the `renv` package.

#### Dockerfile

A `Dockerfile` is a set of instructions that Docker reads from to build a container. You can read the [reference](https://docs.docker.com/engine/reference/builder/) to get a better idea. Here's what mine looks like:

{{< code language="docker" expand="Show" collapse="Hide" isCollapsed="false" >}}
FROM rocker/geospatial:4.0.2

RUN apt-get -y update && apt-get -y upgrade

# Setup renv
RUN Rscript -e 'install.packages("renv")'
RUN Rscript -e 'renv::consent(provided = TRUE)'

# Clone app from GitHub and set WORKDIR
WORKDIR ~
RUN git clone https://github.com/program--/covid-tracker.git
WORKDIR covid-tracker

# Setup cronjob to download COVID-19 data from NYTimes' repo every day
RUN apt-get install -y cron
RUN service cron start
RUN (crontab -l 2>/dev/null ; echo "0 5 * * * /usr/bin/wget https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-counties.csv -O /covid-tracker/data/us-counties.csv") | crontab

# Download packages (workaround for build memory issues) and run app
CMD  cron && Rscript -e 'renv::restore()' && Rscript app.R
{{< /code >}}

Some things to note here are:

- [`FROM`](https://docs.docker.com/engine/reference/builder/#from): Specifies building an image with a *base image*. 
- [`RUN`](https://docs.docker.com/engine/reference/builder/#run): Specifies a command to run via `Bash`
- [`WORKDIR`](https://docs.docker.com/engine/reference/builder/#workdir): Sets the working directory for the commands following its call.
- [`CMD`](https://docs.docker.com/engine/reference/builder/#cmd): Specifies the command to run when the container is deployed.

Since my app is a GIS application, there's some specific packages (i.e. `sf`, `PROJ`, `GDAL`, etc.) that are notoriously difficult to install on containers. You can find a bunch of R-based containers from **rocker** via either [DockerHub](https://hub.docker.com/u/rocker) or [GitHub](https://github.com/rocker-org/rocker). For example, if you need a `tidyverse`-based container, [they have one](https://hub.docker.com/r/rocker/tidyverse).

Now, if you have any extra dependencies, I prefer to use `renv`, which is a dependency manager for R based on `packrat`. We need to ensure this is installed and it is usable on both your *local machine* and the *container*. I'll let you figure out the local machine part, but in our Dockerfile, we want to run a few commands to install it:

{{< code language="docker" expand="Show" collapse="Hide" isCollapsed="false" >}}
# Setup renv
RUN Rscript -e 'install.packages("renv")'
RUN Rscript -e 'renv::consent(provided = TRUE)'
{{< /code >}}

These two commands with (1) install `renv`, and (2) consent to using it. You'll also notice in the `CMD` portion of the Dockerfile, I added

```
... Rscript -e 'renv::restore()' ...
```

This tells `renv` to ensure all dependencies are installed in our container based on the `renv.lock` file in our Dash app directory. This should be automatically generated when you install and run `renv` on your local machine. If you choose *not* to use `renv`, you need ot make sure you install any packages you use, **including Dash**. You can do this by adding:

{{< code language="docker" expand="Show" collapse="Hide" isCollapsed="false" >}}
RUN Rscript -e 'install.packages('dash')
RUN Rscript -e 'install.packages('...')
{{< /code >}}

to your Dockerfile, where `...` is any other packages your app depends on.

Then, after `renv` is installed or your dependencies are sorted out, we need to make sure we grab our Dash app from its repository *(Make sure you actually have the repository created)*.

In your Dockerfile, we can specify the `WORKDIR` of our app. In mine, I wanted to just clone it into the user's home directory via the `WORKDIR ~` command. Then, we can run the `git clone` command, which downloads our repository to the `~/[Repository Name]` directory. Now, we just need to set our `WORKDIR` in this directory with `WORKDIR [Repository Name]`

Finally, we can set up our `CMD` line, which is what commands are run on deployment. At the very least, we need to just add

{{< code language="docker" expand="Show" collapse="Hide" isCollapsed="false" >}}
CMD Rscript app.R
{{< /code >}}

This command runs the Dash app. If your app requires more memory, it might be beneficial to wait to install dependencies until the container is built (like in my case). In this case, you can set your `CMD` line to

{{< code language="docker" expand="Show" collapse="Hide" isCollapsed="false" >}}
CMD Rscript -e 'renv::restore()' && Rscript app.R
{{< /code >}}

which will call `renv::restore()`, then `app.R`. Moreover, you can setup `cronjobs` like I have. If you're unsure what that is, I'd recommend getting more familiar via Google or using *nix before dealing with it.

At this point, our Dockerfile should be good! Once we build it, we can monitor the build logs to make sure it builds correctly or adjust accordingly.

#### Dash `app.R`

I won't go into *how* to build a Dash app, so we really only need to [inspect the last line of our code](https://github.com/program--/covid-tracker/blob/master/app.R) to make sure we can deploy our app:

{{< code language="r" expand="Show" collapse="Hide" isCollapsed="false" >}}
app$run_server(
    host = '0.0.0.0',
    port = Sys.getenv('PORT', 8050),
    serve_locally = FALSE
)
{{< /code >}}

> **Note: `app` should be assigned if you're building with Dash via:**
> 
> `app <- Dash$new()`

Remember that Dash is built on top of `Firey`, a web framework for R. So, this is where we would specify our arguments to Firey about how we want to run our server.

- `host = '0.0.0.0'` - Sets the server to listen on all addresses
- `port = Sys.getenv('PORT', 8050)` - Sets the port of the server to 8050.
- `serve_locally = FALSE` - Whether to serve HTML dependencies locally or remotely.

Now, remember that it's important to consider your `port`, since Dash will default to your Firey server to 8050 if you don't specify. This is less important if you're running Docker behind a reverse proxy (i.e. [NGINX Reverse Proxy](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/)), but if you aren't, then note that you'll need to have your port set to `80` or, preferably, `443`. If you aren't super familiar with ports, know that `80` will serve your app over `http`, which sends traffic as plaintext, whereas port `443` will serve your app with `https`, but also requires an SSL certificate signed by a CA (you can get one for free from [Let's Encrypt](https://letsencrypt.org/)). I won't go into how to install an SSL cert, but there's plenty of tutorials online.

## Now what?

Now that your Dockerfile is completed, and your Dash app is ready to serve correctly, you're pretty much done! You'll have to make sure your backend networking is configured correctly for open-facing applications, and then deploy your container!