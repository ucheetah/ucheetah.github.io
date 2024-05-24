---
layout: post
title: NASA Exoplanet - Visualization and Clustering
date: 2024-05-23
tags: [projects, tech, data, visualization]
image: exoplanet_distance_histogram.png
github: https://github.com/ucheetah/exoplanet-viz-cluster
---


# Visualization and Clustering with NASA Exoplanet Data

This project grabs from the [**NASA exoplanets archive**](https://exoplanetarchive.ipac.caltech.edu/index.html), a collaboration between Caltech and NASA under its Exoplanet Exploration Program. I'm drawing from the **[Planetary Systems](https://exoplanetarchive.ipac.caltech.edu/docs/API_PS_columns.html)** dataset, which provides in-depth data on every confirmed exoplanet known to astronomists to date. The table contains one row per planet per reference and collects data such as its radius and mass, distance, stellar systems.

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
  <img src="https://github.com/ucheetah/exoplanet-viz-cluster/blob/main/nasa_exoplanet_homepage.png" width = "550" height = "400" alt="NASA Homepage" style="border: 2px solid black; border-radius: 5px;">
</p>


## Collecting and cleaning

From the parameter table documentation for this dataset I chose a small set of values from the dataset that are likely to be of biggest interest. The variable in parentheses indicate it's name in the original table:

*   **Exoplanet characteristics**: Planet name (`pl_name`), Planet Radius [Earth Radius] (`pl_rade`), Planet Mass [Earth Mass] (`pl_masse`), Distance [pc] (Distance to the planetary system in units of parsecs) (`sy_dist`)
*   **Stellar system**: Number of Stars in system (`sy_snum`), Number of Planets (in the planetary system) (`sy_pnum`)
*   **Star characteristics**: Stellar Effective Temperature [K] (`st_teff`), Stellar Radius [Solar Radius] (`st_rad`)
*   **Discovery**: Discovery Year (`disc_year`)

### Querying data

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

### Missing value analysis and outlier handling

I started with tracking missing values in the columns I've grabbed.


<p align="center">
  <img src="https://github.com/ucheetah/ucheetah.github.io/assets/img/2024-05-24-exoplanets-1" width = "550" height = "400" alt="NASA Homepage" style="border: 2px solid black; border-radius: 5px;">
</p>






