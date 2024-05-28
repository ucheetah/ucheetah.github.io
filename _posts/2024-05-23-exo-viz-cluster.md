---
layout: post
title: Beyond our World - Visualization, Clustering and Analysis of NASA Exoplanet Archive Data
date: 2024-05-23
tags: [projects, tech, data, visualization]
image: 2024-05-24-exoplanets-3.png
github: https://github.com/ucheetah/exoplanet-viz-cluster
---
This project grabs from the [**NASA exoplanets archive**](https://exoplanetarchive.ipac.caltech.edu/index.html), a collaboration between Caltech and NASA under its Exoplanet Exploration Program. 

I'm drawing from the **[Planetary Systems](https://exoplanetarchive.ipac.caltech.edu/docs/API_PS_columns.html)** dataset, which provides in-depth data on every confirmed exoplanet known to astronomists to date. The table contains one row per planet per reference and collects data such as its radius and mass, distance, stellar systems.

[video link](https://youtu.be/iWowJBRMtpc?t=90s)

<ul>
  <li> <strong>Languages and packages</strong>: one humble SQL query, Python (pandas, matplotlib, seaborn, scikit-learn)</li>
  <li> <strong>Techniques </strong>: data querying, data cleaning, missing value detection, outlier handling, visualization, machine learning (clustering)</li>
</ul>

I have a few goals associated with this project:
<ol>
  <li>Query and collect current data from NASA exoplanet archive's API;</li>
  <li>Perform exploratory data analysis on select exoplanet features and visualize them for analysis using <strong><code>matplotlib</code></strong> and <strong><code>seaborn</code></strong> ;</li>
  <li>Employ a clustering algorithm on the exoplanets using <strong><code>scikit-learn</code></strong> in hopes of generating groups that resemble existing exoplanet classifications (gas giants, terrestrials);</li>
  <li>Explore planet habitability using accepted astronomical science, such as stellar luminosity and star-planet distance.</li>
</ol>

<p align="center">
  <img src="/assets/img/nasa_exoplanet_homepage.png" width = "550" height = "400" alt="NASA Homepage" style="border: 4px solid darkgray; border-radius: 3px;">
</p>


### Collecting and cleaning

From the parameter table documentation for this dataset I chose a small set of values from the dataset that are likely to be of biggest interest. The variable in parentheses indicate it's name in the original table:

*   **Exoplanet characteristics**: Planet name (`pl_name`), Planet Radius [Earth Radius] (`pl_rade`), Planet Mass [Earth Mass] (`pl_masse`), Distance [pc] (Distance to the planetary system in units of parsecs) (`sy_dist`)
*   **Stellar system**: Number of Stars in system (`sy_snum`), Number of Planets (in the planetary system) (`sy_pnum`)
*   **Star characteristics**: Stellar Effective Temperature [K] (`st_teff`), Stellar Radius [Solar Radius] (`st_rad`)
*   **Discovery**: Discovery Year (`disc_year`)

#### Querying data

To query this data we use the industry standard **Table Access Protocol (TAP)** which uses **AQDL (Astronomical Query Data Language)** - akin to SQL for astronomical databases - as queries.

*   Use Python's `requests` package to access NASA data using TAP protocol. I've written a short SQL (technically AQDL) queries which paired with `requests` grabs these columns:

```
  SELECT pl_name, sy_dist, sy_snum, sy_pnum, disc_year, pl_rade,pl_masse,st_teff, st_rad
  FROM ps
```
To fit it into a [TAP protocol](https://exoplanetarchive.ipac.caltech.edu/docs/TAP/usingTAP.html) we simply need to make a few adjustments such as remove additional spacing, add `+` between the major SQL statements. Full procedure:

*   Write data as a CSV into MyDrive.
*   Call CSV into notebook.

 ```
        request_csv = requests.get("https://exoplanetarchive.ipac.caltech.edu/TAP/sync?query=select+pl_name,sy_dist,sy_snum,sy_pnum,disc_year,pl_rade,pl_masse,st_teff,st_rad+from+ps&format=csv")

        with open('/content/gdrive/MyDrive/exoplanet_data.csv', 'w') as f:
          f.write(request_csv.text)
```

#### Missing value analysis and outlier handling

I started with tracking missing values in the columns I've grabbed.

---
<div style="height: 20px;"></div>
<p align="center">
<a href="/assets/img/2024-05-24-exoplanets-1.png">
  <img src="/assets/img/2024-05-24-exoplanets-1.png" width = "800" height = "400" alt="NASA Homepage" style="border: 4px solid darkgray; border-radius: 3px;">
</a>
</p>

<div style="height: 20px;"></div>
---
<div style="height: 20px;"></div>

<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-2.png">
<img src="/assets/img/2024-05-24-exoplanets-2.png" width = "900" height = "450" alt="NASA Homepage" style="border: 4px solid darkgray; border-radius: 3px;">
  </a>
</p>

---
#### Comparing radius and mass

<div style="height: 20px;"></div>
<p align="center">
<img src="/assets/img/2024-05-24-exoplanets-3.png" width = "1000" height = "600" alt="NASA Homepage" style="border: 4px solid darkgray; border-radius: 3px;">
</p>



Observations:
- Linear relationship between planet mass and the log of planet radius.

This is sensible - 
Howver planets differ greatly in their compositions and densities so we would expect there to be quite a bit of fluctuation. 

---

#### Distance from Earth

We next track exoplanet distance from Earth. We'll generate a histogram and cumulative distribution function to observe the distribution of values.

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-4.png">
  <img src="/assets/img/2024-05-24-exoplanets-4.png" width = "800" height = "500" alt="NASA Homepage" style="border: 4px solid darkgray; border-radius: 3px;">
  </a>
</p>

**Observations:**
- Notice that close to 90% of known exoplanets are within the ten thousand light year range.
- Significant drop around the 4000 light year mark.

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-5.png">
  <img src="/assets/img/2024-05-24-exoplanets-5.png" width = "800" height = "500" alt="NASA Homepage" style="border: 4px solid darkgray; border-radius: 3px;">
  </a>
</p>

Plotting a CDF of exolanet distance from Earth, allowing us to better evaluate how planet distances compare to one another.

**Observations:**
- Highly asymptotic behavior - small number as the distance surpasses 4000 light years.

The main takeaway is perhaps expected - planet taper off with distance. This is likely due to a few reasons:
- perhaps there is a significant drop in measurement devices.
- sampling bias - closer planets are given more priority
- there are less planers further - unlikely




    







