---
layout: post
title: Worlds Beyond - Visualization and Clustering with NASA Exoplanet Data
date: 2024-05-23
tags: [projects, tech, data, visualization]
image: 2024-05-24-exoplanets-cover.png
github: https://github.com/ucheetah/exoplanet-viz-cluster
---

<em> This is the first of my new website where I look forward to sharing more of my work in data science and data analysis. </em>

---

Exoplanet are interesting because their study is the gateway to discovering and confirming the existence of extraterrestrial life. Over the past decade astronomy has seen an explosion in new planets. With the introduction the James Webb Telescope,the most complex and largest that NASA has ever launched into space, and it's allocation a significant portion of it's study to the discovery of exoplanets, the results are expected to be seismic.  

This project grabs from the [**NASA exoplanets archive**](https://exoplanetarchive.ipac.caltech.edu/index.html){:target="_blank"}, a collaboration between Caltech and NASA under its Exoplanet Exploration Program. 

I'm drawing from the **[Planetary Systems](https://exoplanetarchive.ipac.caltech.edu/docs/API_PS_columns.html)** dataset, which provides in-depth data on every confirmed exoplanet known to astronomists to date. The table contains one row per planet per reference and collects data such as its radius and mass, distance, stellar systems.

<ul>
  <li> <strong>Languages and packages</strong>: Python (pandas, matplotlib, seaborn, scikit-learn)</li>, SQL (one humble query)
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

#### Querying data

To query this data we use the industry standard **Table Access Protocol (TAP)** which uses **AQDL (Astronomical Query Data Language)** - akin to SQL for astronomical databases - as queries.

*   Use Python's `requests` package to access NASA data using TAP protocol. I've written a short SQL (technically AQDL) queries which paired with `requests` grabs these columns:

```
  SELECT pl_name, sy_dist, pl_rade, pl_masse
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


After removing the first few columns we obtain a preliminary look at our first data points.

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-C.png">
  <img src="/assets/img/2024-05-24-exoplanets-C.png" width="320" height="120" alt="Graph C" style="border: 4px solid darkgray; border-radius: 3px;">
  </a>
</p>


#### Discovery method
For this I must credit the __ from in a recent lecture who interestingly expressed how the growth of discovery of the planets is comparable to the rapid development of internet technology and telephones. So as a domain, exoplanet science is progressing at an extremely rapid pace, and this graph demonstrates this progress.

I've calculated and graphed the thre most successful exoplanet discovery methods there is. 

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-J.png">
  <img src="/assets/img/2024-05-24-exoplanets-J.png" width="1440" height="960" alt="Graph J" style="border: 4px solid darkgray; border-radius: 3px;">
<div style="height: 20px;"></div>

<em> Observations </em>
<ul>
  <li>There is clearly a ____ increase in discoveries.</li>
  <li> Some years, such as 2016, have been enormous years for exoplanet discoveries. </li>
  <li>The **transit method** is clearly the most successful discovery method. It exploits the occurence of a planet passing between a star and the observer (Earth). The light from the star will be altereted by the planet's presense. The amount of dimming occured can be used to make conclusions about the planet's characeristics like mass, radiius and eve atmospheric conditions.
</li>

#### Distance from Earth

We next track exoplanet distance from Earth. We can first generate a histogram (technically this is a lollipop plot) of exoplanets respective distance to Earth:

<div style="height: 20px;"></div>

<div style="height: 20px;"></div>
---
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-D.png">
  <img src="/assets/img/2024-05-24-exoplanets-D.png" width="1200" height="600" alt="Graph D" style="border: 4px solid darkgray; border-radius: 3px;">
  </a>
</p>
---

Plotting a CDF of exolanet distance from Earth, allowing us to better evaluate how planet distances compare to one another.

<div style="height: 20px;"></div>
---
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-E.png">
  <img src="/assets/img/2024-05-24-exoplanets-E.png" width="1200" height="600" alt="Graph E" style="border: 4px solid darkgray; border-radius: 3px;">
  </a>
</p>
---

**Observations:**
- Close to 90% of known exoplanets are within the ten thousand light year range.
- In the CDF we notice a highly asymptotic behavior - small number as the distance surpasses 4000 light years.
- A small number of planets have been accessed beyond 10,000 light years remarkably.

<div style="height: 20px;"></div>

The main takeaway is perhaps expected - planet taper off with distance. This is likely due to a few reasons:
- perhaps there is a significant drop in measurement devices.
- sampling bias - closer planets are given more priority
- there are less planers further - unlikely

---

#### Comparing planet mass and radius
<div style="height: 20px;"></div>
Observing the disparity of planet size and mass can allow us to create a better demographic picture of the planets in our observed universe. Next we will take a look at exoplanet radius and mass. The following is a scatterplot of mass and radius. Note that the scatterplot colors are defined in terms of mass, and the mass is in a logarithmic scale.
---
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-F.png">
  <img src="/assets/img/2024-05-24-exoplanets-F.png" width="910.65" height="720" alt="Graph F" style="border: 4px solid darkgray; border-radius: 3px;">
  </a>
</p>
---

**Observations:**
- Linear relationship between planet mass and the log of planet radius.
- Appear to be two main groups of data, one with smaller radius and mass, another with larger.
- A few outliers on the ends.
- radius and mass appear to correlate. 
- Significant fluctuations in planet mass and radius - the heaviest planet is ______x heavier than Earth. This may be understood by the fact that radius and mass 

However planets differ greatly in their compositions and densities so we would expect there to be quite a bit of fluctuation. 

---

#### Machine learning model - clustering

To determine the number of categories (clusters) we want to develop, we will use the silhouette score method. You may consult my script for a better understanding of this method. The following returns the silhouette score. High scores indicate good clustering results, bad scores indicate bad results.

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-G.png">
  <img src="/assets/img/2024-05-24-exoplanets-G.png" width="640" height="240" alt="Graph G" style="border: 4px solid darkgray; border-radius: 3px;">
  </a>
</p>

**Observation(s):**
- Clearly the most favorable score is 4, so we will divide our exoplanet data into four categories in attempts to match them with existing categories.

We run a kmeans model using scikit learn and add the clusters it has generate to our data. In the following graph we display those results, with each cluster differing in color and 

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-H.png">
  <img src="/assets/img/2024-05-24-exoplanets-H.png" width="910.65" height="720" alt="Graph H" style="border: 4px solid darkgray; border-radius: 3px;">
  </a>
</p>
**Observation(s):**
- Clearly the most favorable score is 4, so we will divide our exoplanet data into four categories in attempts to match them with existing categories.



<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-I.png">
  <img src="/assets/img/2024-05-24-exoplanets-I.png" width="768" height="576" alt="Graph I" style="border: 4px solid darkgray; border-radius: 3px;">
  </a>
</p>
