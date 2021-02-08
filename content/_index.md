---
layout: "index"
framed: true
---

<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.1/css/all.min.css" integrity="sha512-+4zCK9k+qNFUR5X+cKL9EIR+ZOhtIloNl9GIKS57V1MyNsYpYcUrUeQc9vNfzsWfV28IaLL3i96P9sdNyeRssA==" crossorigin="anonymous" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.1/js/all.min.js" integrity="sha512-F5QTlBqZlvuBEs9LQPqc1iZv2UMxcVXezbHzomzS6Df4MZMClge/8+gXrKw2fl5ysdk4rWjR0vKS7NNkfymaBQ==" crossorigin="anonymous"></script>

<style>
    .menu-icon {
        margin: 0 50px 0 50px;
        color: #78E2A0;
    }

    .menu-icon:hover {
        color: #6ccc91;
    }

    .menu-parent {
        display: flex;
        flex-direction: row;
        background: repeating-linear-gradient(90deg, #2b2f3b, #2b2f3b 2px, transparent 0, transparent 10px);
        z-index: -1;
        padding: 20px 0 20px 0;
        text-align: center;
    }

    .menu-container {
        display: flex;
        flex-direction: column;
        flex: 1
    }

    @media (max-width: 800px) {
        .menu-parent {
            flex-direction: column;
        }
    }

    .menu-text {
        font-size: 1.4rem;
    }

</style>

<div class="menu-parent">

<div class="menu-container">
<a href="https://github.com/program--">
<i class="menu-icon fab fa-github-square fa-5x"></i>
</a>
<a href="https://github.com/program--">
<span class="menu-text">GitHub</span>
</a>
</div>

<div class="menu-container">
<a href="/assets/jsingh_resume.pdf">
<i class="menu-icon far fa-file-alt fa-5x" data-fa-transform="shrink-6" data-fa-mask="fas fa-square"></i>
</a>
<a href="/assets/jsingh_resume.pdf">
<span class="menu-text">Resume</span>
</a>
</div>

<div class="menu-container">
<a href="https://geog176a.justinsingh.me">
<i class="menu-icon fas fa-globe-americas fa-5x" data-fa-transform="shrink-6" data-fa-mask="fas fa-square"></i>
</a>
<a href="https://geog176a.justinsingh.me">
<span class="menu-text">GIS</span>
</a>
</div>

<div class="menu-container">
<a href="mailto:justin@justinsingh.me">
<i class="menu-icon fas fa-envelope-square fa-5x"></i>
</a>
<a href="mailto:justin@justinsingh.me">
<span class="menu-text">Contact</span>
</a>
</div>

</div>