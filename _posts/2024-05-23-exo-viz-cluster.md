---
layout: post
title: Worlds Beyond - Visualization and Clustering of NASA Exoplanet Data
date: 2024-05-23
tags: [projects, tech, data science, visualization, machine learning, kmeans]
image: 2024-05-24-exoplanets-cover.png
github: https://github.com/ucheetah/exoplanet-viz-cluster
---

<p align="center"> <em> This post marks the beginning of my new personal website, where I'm excited to share more about my work in data science, data analysis and  ideas at large that inspire me.</em> </p>

<hr>
<div style="height: 20px;"></div>

The study of exoplanets - planets beyond our Solar System - is a crucial gateway to discovering and confirming the existence of extraterrestrial life. Over the past decade the scientific community has seen an explosion in our capabilities to gather fine-grained data on the observable universe, thanks to advancements into astronomical science like the James Webb Telescope - the most complex and largest telescope launched into space - which began it's work in 2021. Many of these new technologies, including the James Webb, are used to discovery of exoplanets. As such the discussion and interest on exoplanets is only expected grow in the coming years. 

This project peers into exoplanet science through the lens of data. I grab from the <strong><a href="https://exoplanetarchive.ipac.caltech.edu/docs/API_PS_columns.html" target="_blank" style="color: #573259;"> Planetary Systems</a></strong> dataset provided by the <strong><a href="https://exoplanetarchive.ipac.caltech.edu/index.html" target="_blank" style="color: #573259;">NASA exoplanets archive</a> </strong> on which I'll visualize and conduct a k-means clustering algorithm.

<h5 align="center"> Project summary</h5>
<div style="height: 10px;"></div>

<table align="center">
  <tr>
    <td align="center"><strong> Goals </strong> </td>
    <td>
      <br>
    <ul>
      <li>Query and collect data from NASA exoplanet archive;</li>
      <li>Perform exploratory data analysis on exoplanet features and visualize them for analysis; and</li>
      <li>Employ kmeans clustering on the exoplanets comparing these to existing exoplanet classifications.</li>
    </ul>
    </td>
  </tr>
  <tr>
    <td><strong>Languages</strong></td>
    <td>Python (pandas, numpy, matplotlib, seaborn, scikit-learn), SQL (one humble query)</td>
  </tr>
  <tr>
    <td><strong>Techniques</strong></td>
    <td>Data querying, data cleaning, missing value detection/outlier handling, visualization, machine learning (clustering)</td>
  </tr>
  <tr>
    <td><strong>Project GitHub</strong></td>
    <td> <a href="https://github.com/ucheetah/exoplanet-viz-cluster/tree/main">GitHub Repo - exoplanet-viz-cluster</a> </td>
  </tr>
</table>

<br>

<figure style="text-align: center;">
   <a href = "https://exoplanetarchive.ipac.caltech.edu/index.html" target="_blank">
      <img src="/assets/img/nasa_exoplanet_homepage.png" width = "550" height = "400" alt="NASA Homepage" style="border: 4px solid darkgray; border-radius: 3px;">
    </a>
      <figcaption>NASA Exoplanet Archive Website</figcaption> 
</figure>
<div style="height: 20px;"></div>

<h3>Data collection and cleaning</h3> 
<div style="height: 20px;"></div>

I will draw data from the NASA exoplanets archive, a collaboration between Caltech and NASA under its Exoplanet Exploration Program.
From the parameter table documentation for this dataset I chose a small set of values from the dataset that are likely to be of biggest interest. The variable in parentheses indicate it's name in the original table:

<ul>
  <li><strong>Planet name</strong> as <code>pl_name</code></li>
  <li><strong>Planet radius</strong> as <code>pl_rade</code></li>
  <li><strong>Planet mass</strong> as <code>pl_masse</code></li>
  <li><strong>Distance from Earth</strong> as <code>sy_dist</code></li>
 <li><strong>Discovery method</strong> as <code>pl_masse</code></li>
  <li><strong>Discovery year</strong> as <code>disc_year</code></li>
</ul>

<div style="height: 20px;"></div>
<h5>Querying data</h5>
<div style="height: 20px;"></div>

To query this data we use the industry standard **Table Access Protocol (TAP)** which uses **AQDL (Astronomical Query Data Language)** - akin to SQL for astronomical databases - as queries. Use Python's <code>requests</code> package to access NASA data using TAP protocol. I've written a short SQL (technically AQDL) queries which paired with <code>requests</code> grabs these columns:

<p align="center">
  <code>
  <strong>SELECT pl_name, sy_dist, pl_rade, pl_masse FROM ps</strong>
</code>
</p>

To fit it into a <strong><a href="https://exoplanetarchive.ipac.caltech.edu/docs/TAP/usingTAP.html" target="_blank" style="color: #573259;">TAP protocol</a></strong> we simply need to make a few adjustments such as remove additional spacing, add `+` between the major SQL statements. In my process I ingested the data as a CSV into MyDrive to then call it into my notebook.

<div style="height: 20px;"></div>
<h5>Raw data summary</h5>
<div style="height: 20px;"></div>

After removing the first few columns we obtain a preliminary look at our first data points. The format of these tables is drawn from <strong><a href="https://www.sonofacorner.com/beautiful-tables/" target="_blank" style="color: #573259;"> Beautiful Tables in Matplotlib, a Tutorial</a></strong> and <strong><a href="https://matplotlib.org/matplotblog/posts/how-to-create-custom-tables/?ref=sonofacorner.com" target="_blank" style="color: #573259;">How to create custom tables</a></strong>.

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-C.svg" target="_blank">
  <img src="/assets/img/2024-05-24-exoplanets-C.svg"  width="600" alt="Graph C" style="border: 4px solid darkgray; border-radius: 3px;">
  </a>
</p>
<div style="height: 20px;"></div>

<h3>Visualization and Analysis</h3>
<div style="height: 20px;"></div>
<h4>Discovery method</h4>
<div style="height: 20px;"></div>

Taking a look at how are planets are discovered reveals a lot about the underlying science. Exoplanets are discovered in many ways but a few ways dominate in their success. I've tracked the growrth of the three most successful exoplanet discovery methods graphed since the start of the century.

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-J.svg" target="_blank">
  <img src="/assets/img/2024-05-24-exoplanets-J.svg" width="1440" height="960" alt="Graph J" style="border: 4px solid darkgray; border-radius: 3px;">
  </a>
<div style="height: 20px;"></div>

<strong> Observations </strong>
<ul>
  <li>There's a clear increase in discoveries over the past ten years.</li>
  <li>Certain years, such as 2016, have seen enormous numbers of exoplanet discoveries. </li>
  <li>The <strong>transit method</strong> is clearly the most successful discovery method.</li>
</ul>

<p> The <strong>transit method</strong> exploits the occurence of a planet passing between a star and the observer (Earth). The light from the star will be altereted by the planet's presense. The amount of dimming occured can be used to make conclusions about the planet's characeristics like mass, radiius and eve atmospheric conditions.</p> 

<p> Observational cosmologist Chris Impey mentioned in a recent <a href="https://www.youtube.com/watch?v=fbRfJTiQYtA&ab_channel=TheRoyalInstitution" target="_blank" style="color: #573259;">lecture</a> that the growth of discovery of the planets is comparable to the development of internet technology and telephones seen during the Dotcom bubble. As a domain, exoplanet science is progressing at a very rapid pace.</p> 

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

<strong>Observation(s):</strong>
<ul>
  <li> The distribution is predicatble where the most planets are closer.</li>
  <li> Fairly significant findings of planets up to 3500 light years.</li>
  <li> A small number of planets have still been been accessed at 10,000 light years away remarkably.</li>
</ul>

<p> We can also better evaluate the distribution of planets as a whole through a cumulative distribution function (CDF):</p>

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-E.svg" target="_blank">
  <img src="/assets/img/2024-05-24-exoplanets-E.svg" width="1200" height="600" alt="Graph E" style="border: 4px solid darkgray; border-radius: 3px;">
  </a>
</p>
<div style="height: 20px;"></div>

<strong>Observation(s):</strong>
<ul>
  <li> Close to 90% of known exoplanets are within the ten thousand light year range. </li>
  <li> In the CDF we notice a highly asymptotic behavior - small number as the distance surpasses 4000 light years. </li>
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

<strong>Observation(s):</strong>

<ul>
<li> Linear relationship between planet mass and the log of planet radius.</li>
<li> Appear to be two main groups of data, one with smaller radius and mass, another with larger.</li>
<li> A few outliers on the ends.</li>
<li> radius and mass appear to correlate.</li>
<li> Significant fluctuations in planet mass and radius - the heaviest planet is 10000x heavier than Earth. This may be understood by the fact that mass increases exponentially with respect to radius. </li>
</ul>
<div style="height: 20px;"></div>

However planets differ greatly in their compositions and densities so we would expect there to be quite a bit of fluctuation. 

<div style="height: 20px;"></div>

<h3> K-means clustering</h3>
<div style="height: 20px;"></div>

To determine the number of categories (clusters) we want to develop, we will use the <strong>silhouette score</strong> method. You may consult my script for a better understanding of this method. High scores indicate good clustering results, bad scores indicate bad results.

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

<div style="height: 20px;"></div>
<strong>Observation(s):</strong>
<ul>
<li>Clearly the most favorable score is 4, so we will divide our exoplanet data into four categories in attempts to match them with existing categories.</li>
</ul>


<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-I.png" target="_blank">
  <img src="/assets/img/2024-05-24-exoplanets-I.png" width="700" alt="Graph I" style="border: 4px solid darkgray; border-radius: 3px;">
  </a>
</p>
<div style="height: 20px;"></div>

<strong>Observation(s):</strong>
<ul>
<li>There doesn't appear to be a strong correlation between mass/radius and distance. This makes sense - we wouldn't expect composition and components of the planets to depend on their distance from Earth</li>
</ul>

If worked has piqued your interest on exoplanet science, here are a few resources I appreciate to keep reading on the matter:
<ul>
<li><a href="https://www.quantamagazine.org/the-best-neighborhoods-for-starting-a-life-in-the-galaxy-20240124/" target="_blank" style="color: #573259;">The Best Neighborhoods for Starting a Life in the Galaxy</a></li>
</ul>

