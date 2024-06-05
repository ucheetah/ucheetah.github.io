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

Exoplanet are interesting because their study is the gateway to discovering and confirming the existence of extraterrestrial life. Over the past decade astronomy has seen an explosion in new planets. With the introduction the James Webb Telescope - the most complex and largest telescope launched into space - allocating a significant portion of it's study to the discovery of exoplanets, the results on our knowledge of exoplanets are expected to be seismic. 

In this project I grab a few key characteristics known of planets nearby and perform visualization and a k-means clustering algorithm.

This project grabs from the <a href="https://exoplanetarchive.ipac.caltech.edu/index.html" target="_blank">NASA exoplanets archive</a>, a collaboration between Caltech and NASA under its Exoplanet Exploration Program. I'm drawing from the **[Planetary Systems](https://exoplanetarchive.ipac.caltech.edu/docs/API_PS_columns.html)** dataset which records every confirmed exoplanet known to astronomists to date.


<table>
  <caption>
    <strong> Project summary </strong>
  </caption>
  <tr>
    <td>Language and packages</td>
    <td>Python (pandas, numpy, matplotlib, seaborn, scikit-learn)</li>, SQL (one humble query)</td>
  </tr>
  <tr>
    <td>Techniques</td>
    <td>Data querying, data cleaning, missing value detection/outlier handling, visualization, machine learning (clustering)</td>
  </tr>
</table>

I have a few goals associated with this project:
<ol>
  <li>Query and collect current data from NASA exoplanet archive's API;</li>
  <li>Perform exploratory data analysis on select exoplanet features and visualize them for analysis using <strong><code>matplotlib</code></strong> and <code>seaborn</code> ;</li>
  <li>Employ a clustering algorithm on the exoplanets using <strong><code>scikit-learn</code></strong> in hopes of generating groups that resemble existing exoplanet classifications (gas giants, terrestrials);</li>
  <li>Explore planet habitability using accepted astronomical science, such as stellar luminosity and star-planet distance.</li>
</ol>
<br>
<div style="height: 20px;"></div>
<p align="center">
  <a href = "https://exoplanetarchive.ipac.caltech.edu/index.html" target="_blank">
  <img src="/assets/img/nasa_exoplanet_homepage.png" width = "550" height = "400" alt="NASA Homepage" style="border: 4px solid darkgray; border-radius: 3px;">
  </a>
</p>
<div style="height: 20px;"></div>

<h3>Data collection and cleaning</h3> 
<div style="height: 20px;"></div>

From the parameter table documentation for this dataset I chose a small set of values from the dataset that are likely to be of biggest interest. The variable in parentheses indicate it's name in the original table:

<ul>
  <li><strong>Exoplanet characteristics</strong>: Planet name (`pl_name`), Planet Radius [Earth Radius] (`pl_rade`), Planet Mass [Earth Mass] (`pl_masse`), Distance [pc] (Distance to the planetary system in units of parsecs) (`sy_dist`)</li>
</ul> 

<div style="height: 20px;"></div>
<h4>Querying data</h4>
<div style="height: 20px;"></div>

To query this data we use the industry standard **Table Access Protocol (TAP)** which uses **AQDL (Astronomical Query Data Language)** - akin to SQL for astronomical databases - as queries.

*   Use Python's `requests` package to access NASA data using TAP protocol. I've written a short SQL (technically AQDL) queries which paired with `requests` grabs these columns:

<p>
  <code>
  SELECT pl_name, sy_dist, pl_rade, pl_masse <br>
  FROM ps
</code>
</p>

To fit it into a [TAP protocol](https://exoplanetarchive.ipac.caltech.edu/docs/TAP/usingTAP.html) we simply need to make a few adjustments such as remove additional spacing, add `+` between the major SQL statements. In my process I ingested the data as a CSV into MyDrive to then call it into my notebook.

 <code style="color: darkgrey;">
request_csv = requests.get("https://exoplanetarchive.ipac.caltech.edu/TAP/sync?query=select+pl_name,sy_dist,sy_snum,sy_pnum,disc_year,pl_rade,pl_masse,st_teff,st_rad+from+ps&format=csv") <br>
with open('/content/gdrive/MyDrive/exoplanet_data.csv', 'w') as f:
f.write(request_csv.text)
</code>


After removing the first few columns we obtain a preliminary look at our first data points. The format of these tables is drawn from _____ and _____

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-C.svg" target="_blank">
  <img src="/assets/img/2024-05-24-exoplanets-C.svg"  width="800" alt="Graph C" style="border: 4px solid darkgray; border-radius: 3px;">
  </a>
</p>
<div style="height: 20px;"></div>

<h4>Discovery method</h4>
<div style="height: 20px;"></div>

For this I must credit the __ from in a recent lecture who interestingly expressed how the growth of discovery of the planets is comparable to the rapid development of internet technology and telephones. So as a domain, exoplanet science is progressing at an extremely rapid pace, and this graph demonstrates this progress.

I've calculated and graphed the thre most successful exoplanet discovery methods there is. 

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-J.svg" target="_blank">
  <img src="/assets/img/2024-05-24-exoplanets-J.svg" width="1440" height="960" alt="Graph J" style="border: 4px solid darkgray; border-radius: 3px;">
  </a>
<div style="height: 20px;"></div>

<strong> Observations </strong>
<ul>
  <li>There is clearly a ____ increase in discoveries.</li>
  <li> Some years, such as 2016, have been enormous years for exoplanet discoveries. </li>
  <li>The **transit method** is clearly the most successful discovery method. It exploits the occurence of a planet passing between a star and the observer (Earth). The light from the star will be altereted by the planet's presense. The amount of dimming occured can be used to make conclusions about the planet's characeristics like mass, radiius and eve atmospheric conditions. </li>
</ul>

<div style="height: 20px;"></div>
<h4>Distance from Earth</h4>
<div style="height: 20px;"></div>

We next track exoplanet distance from Earth. We can first generate a histogram (technically this is a lollipop plot) of exoplanets respective distance to Earth:

<div style="height: 20px;"></div>

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-D.svg" target="_blank">
  <img src="/assets/img/2024-05-24-exoplanets-D.svg" width="1200" height="600" alt="Graph D" style="border: 4px solid darkgray; border-radius: 3px;">
  </a>
</p>
<div style="height: 20px;"></div>

Plotting a CDF of exolanet distance from Earth, allowing us to better evaluate how planet distances compare to one another.

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-E.svg" target="_blank">
  <img src="/assets/img/2024-05-24-exoplanets-E.svg" width="1200" height="600" alt="Graph E" style="border: 4px solid darkgray; border-radius: 3px;">
  </a>
</p>
<div style="height: 20px;"></div>

<strong>Observations:</strong>
<ul>
  <li> Close to 90% of known exoplanets are within the ten thousand light year range. </li>
  <li> In the CDF we notice a highly asymptotic behavior - small number as the distance surpasses 4000 light years. </li>
  <li> A small number of planets have been accessed beyond 10,000 light years remarkably. </li>
</ul>
<div style="height: 20px;"></div>

Unsurprisingly known planets taper off with distance. This is likely due to a few reasons:
- perhaps there is a significant drop in measurement devices.
- sampling bias - closer planets are given more priority
- there are less planers further - unlikely

<div style="height: 20px;"></div>

<h4> Comparing planet mass and radius </h4>

<div style="height: 20px;"></div>
Observing the disparity of planet size and mass can allow us to create a better demographic picture of the planets in our observed universe. Next we will take a look at exoplanet radius and mass. The following is a scatterplot of mass and radius. Note that the scatterplot colors are defined in terms of mass, and the mass is in a logarithmic scale.

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-F.svg" target="_blank">
  <img src="/assets/img/2024-05-24-exoplanets-F.svg" width="910.65" height="720" alt="Graph F" style="border: 4px solid darkgray; border-radius: 3px;">
  </a>
</p>
<div style="height: 20px;"></div>

<strong>Observations:</strong>

<ul>
<li> Linear relationship between planet mass and the log of planet radius.</li>
<li> Appear to be two main groups of data, one with smaller radius and mass, another with larger.</li>
<li> A few outliers on the ends.</li>
<li> radius and mass appear to correlate.</li>
<li> Significant fluctuations in planet mass and radius - the heaviest planet is ______x heavier than Earth. This may be understood by the fact that radius and mass. </li>
</ul>

However planets differ greatly in their compositions and densities so we would expect there to be quite a bit of fluctuation. 

<div style="height: 20px;"></div>

<h4> Machine learning - K-means clustering with `scikit-learn`</h4>

To determine the number of categories (clusters) we want to develop, we will use the silhouette score method. You may consult my script for a better understanding of this method. The following returns the silhouette score. High scores indicate good clustering results, bad scores indicate bad results.

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-G.svg" target="_blank">
  <img src="/assets/img/2024-05-24-exoplanets-G.svg" width="800" alt="Graph G" style="border: 4px solid darkgray; border-radius: 3px;">
  </a>
</p>
<div style="height: 20px;"></div>

<strong>Observation(s): </strong>
<ul>
<li>Clearly the most favorable score is 4, so we will divide our exoplanet data into four categories in attempts to match them with existing categories.</li>
</ul>
<div style="height: 20px;"></div>

We run a kmeans model using scikit learn and add the clusters it has generate to our data. In the following graph we display those results, with each cluster differing in color and 

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-H.svg" target="_blank">
  <img src="/assets/img/2024-05-24-exoplanets-H.svg" width="910.65" height="720" alt="Graph H" style="border: 4px solid darkgray; border-radius: 3px;">
  </a>
</p>
<strong>Observation(s):</strong>
<ul>
<li>Clearly the most favorable score is 4, so we will divide our exoplanet data into four categories in attempts to match them with existing categories.</li>
</ul>


<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-I.png" target="_blank">
  <img src="/assets/img/2024-05-24-exoplanets-I.png" width="1200" alt="Graph I" style="border: 4px solid darkgray; border-radius: 3px;">
  </a>
</p>

