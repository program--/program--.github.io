---
title: "Research & Projects"
---

<style>
.tabs {
    display: flex;
    flex-wrap: wrap;
    margin: auto auto 2rem auto;
    max-width: 700px;
    box-shadow: 0 5px 100px -15px rgba(0,0,0,0.5),  0 0 5px -2px #78E2A0;
}

.input {
    position: absolute;
    opacity: 0;
}

.label {
    width: 100%;
    padding: 20px 30px;
    background: #17171c;
    cursor: pointer;
    font-weight: bold;
    font-size: 18px;
    transition: background 0.1s, color 0.1s;
}

.label:hover {
    background: #1a1b23;
}

.input:focus + .label {
    box-shadow: inset 0px 0px 0px 3px #78E2A0;
    z-index: 1;
}

.input:checked + .label {
    background: color-mod(var(--accent) blend(#1D1E28 98%));
}

@media (min-width: 600px) {
    .label {
        width: auto;
    }
}

.panel {
    display: none;
    padding: 20px 30px 30px;
    background: color-mod(var(--accent) blend(#1D1E28 98%));
}
  
@media (min-width: 600px) {
    .panel {
        order: 99;
    }
}
  
.input:checked + .label + .panel {
    display: block;
}

.rlogo {
    filter: drop-shadow(0px 0px 3px black);
}
</style>

<center><h2>Publications</h2></center>

> Johnson, J.M., Eyelade D., Clarke K.C, **Singh-Mohudpur, J.** (2021) *“Characterizing Reach-level Empirical Roughness Along the National Hydrography Network: Developing DEM-based Synthetic Rating Curves.”*

<center><h2>R Packages</h2></center>

<div class="tabs">
  <input name="tabs" type="radio" id="tab-1" checked="checked" class="input"/>
  <label for="tab-1" class="label">HSClientR</label>
  <div class="panel">
    <h1><a href="https://hsclientr.justinsingh.me">HSClientR</a><a href="https://hsclientr.justinsingh.me"><img class="rlogo" src="/img/hsclientr_logo.png" alt="hsclientr" width=25% align="right" /></a></h1>
    <p>A <b>R</b> package API wrapper for <a href="https://hydroshare.org">HydroShare</a>.</p>
  </div>

  <input name="tabs" type="radio" id="tab-2" class="input"/>
  <label for="tab-2" class="label">plannr</label>
  <div class="panel">
    <h1><a href="https://plannr.justinsingh.me">plannr</a><a href="https://plannr.justinsingh.me"><img class="rlogo" src="/img/plannr_logo.png" alt="plannr" width=25% align="right" /></a></h1>
    <p>A <b>R</b> package for interfacing with <em>Microsoft Planner</em> data.</p>
  </div>

  <input name="tabs" type="radio" id="tab-3" class="input"/>
  <label for="tab-3" class="label">cryptocurr</label>
  <div class="panel">
    <h1><a href="https://cryptocurr.justinsingh.me">cryptocurr</a><a href="https://cryptocurr.justinsingh.me"><img class="rlogo" src="/img/cryptocurr_logo.png" alt="cryptocurr" width=25% align="right" /></a></h1>
    <p>A <b>R</b> package API wrapper for various <em>cryptocurrency</em> exchange platforms.</p>
  </div>
</div>

***

# [COVID-19 Historical Data](https://covid.justinsingh.me)
A **Dash-R** web app built to analyze historical COVID-19 data.

# [R + GIS](https://gis.justinsingh.me/gis)
Using libraries from **tidyverse**, **sf**, **and more...**, I performed
analysis on spatial datasets that covered various topic, such as:

- COVID-19
- Flood Risk Analysis (Remote Sensing)
- Point-in-Polygon Analysis

***