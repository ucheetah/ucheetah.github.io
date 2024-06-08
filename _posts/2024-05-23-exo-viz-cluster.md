---
layout: post
title: Worlds Beyond - Visualizing and Clustering NASA Exoplanet Data
date: 2024-05-23
tags: [projects, tech, data science, visualization, machine learning, kmeans]
image: 2024-05-24-exoplanets-cover.png
github: https://github.com/ucheetah/exoplanet-viz-cluster
---

<p align="center"> <em> This post marks the beginning of my new personal website, where I'll be share my ventures in data science, tech and their intersection with everything else. Hope to share ideas at large that inspire me.</em> </p>

<hr>
<div style="height: 20px;"></div>

This project peers into exoplanet science through the lens of data. I grab from the <strong><a href="https://exoplanetarchive.ipac.caltech.edu/docs/API_PS_columns.html" target="_blank" style="color: #573259;"> Planetary Systems</a></strong> dataset provided by the <strong><a href="https://exoplanetarchive.ipac.caltech.edu/index.html" target="_blank" style="color: #573259;">NASA exoplanets archive</a> </strong> on which I'll visualize and perform unsupervised learning through a k-means clustering algorithm.

<h3 align="center"> Project summary</h3>
<div style="height: 10px;"></div>

<table align="center">
  <tr>
    <td align="center"><strong> Goals </strong> </td>
    <td>
    Query and clean data from NASA exoplanet archive; perform exploratory data analysis and visualize planet features; and employ k-means clustering comparing groups to existing exoplanet classifications.
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
    <td> <a href="https://github.com/ucheetah/exoplanet-viz-cluster/tree/main" target="_blank" style="color: #573259;">GitHub Repo - exoplanet-viz-cluster</a> </td>
  </tr>
</table>

<div style="height: 20px;"></div>
<h3 align="center"> Background</h3>
<div style="height: 20px;"></div>

<p>Our popular conception of space is dominated by stars. Understandably though. Not to get poetic and all, but for as long there's been life on Earth, we've gazed at the night sky. Stars are an intrinsic part of our societies, woven into our religions, and even fill out the awkward dinner conversation. </p>

<p>Planets, on the other hand, just don't get the same notoriety The nine we know well certainly do. But the planets outside our Solar System, what are called <strong>exoplanets</strong> or <strong>extrasolar planets</strong>, do not (aside from the odd <a href="https://earthsky.org/space/vulcan-hd-26965-b-exoplanets-star-trek/">Vulcan</a> reference.)</p>

<p>I would argue that this view is changing, though. Most astronomers recognize something invaluable that the study of exoplanets could offer - confirming the existence of extraterrestrial life. Many experts would say we're guaranteed to find life beyond our Solar System given enough searching and the genuine unlikelihood that life on Earth is unique. But it's one thing to know the probability of extraterrestrial life, and another to confirm; ask <a href="https://www.nytimes.com/2023/07/26/us/politics/ufo-hearing.html">U.S. Congress</a>.</p>
  
<p>Projects such as <strong>China's Earth 2.0 Space mission</strong> or NASA's projected 2040 <strong>Habitable Worlds Observatory</strong> focus squarely on this mission - investigating exoplanets that could host life. Whether these inquiries are driven by pure curiosity or deep-rooted cynicism about our own survivability on this planet, Earth 1.0, is a question for another day.</p>

<div style="height: 20px;"></div>

<h3 align="center">Data collection and cleaning</h3> 
<div style="height: 20px;"></div>

I will draw data from the NASA exoplanets archive, a collaboration between Caltech and NASA under its Exoplanet Exploration Program.
From the parameter table documentation for this dataset I chose a small set of values from the dataset that are likely to be of biggest interest. 

<div style="height: 20px;"></div>

<figure style="text-align: center;">
   <a href = "https://exoplanetarchive.ipac.caltech.edu/index.html" target="_blank">
      <img src="/assets/img/nasa_exoplanet_homepage.png" width = "550" height = "400" alt="NASA Homepage" style="border: 3px solid #573259; border-radius: 3px;">
    </a>
      <figcaption>NASA Exoplanet Archive Website</figcaption> 
</figure>
<div style="height: 20px;"></div>

<div style="height: 20px;"></div>
<h5>Querying data</h5>
<div style="height: 20px;"></div>

To query this data we use the NASA's <strong>Table Access Protocol (TAP)</strong>. I've written a short SQL (technically AQDL) queries which paired with Python's <code>requests</code> package grabs these columns:

<p align="center">
  <code>
  <strong>SELECT pl_name, sy_dist, pl_rade, pl_masse FROM ps</strong>
</code>
</p>

You may consult my notebook for the full process.
<div style="height: 20px;"></div>
<h5>Raw data summary</h5>
<div style="height: 20px;"></div>

 I've grabbed the following planet features:

<ul>
  <li><strong>Planet name</strong> as <code>pl_name</code></li>
  <li><strong>Planet radius</strong> as <code>pl_rade</code></li>
  <li><strong>Planet mass</strong> as <code>pl_masse</code></li>
  <li><strong>Distance from Earth</strong> as <code>sy_dist</code></li>
 <li><strong>Discovery method</strong> as <code>discoverymethod</code></li>
  <li><strong>Discovery year</strong> as <code>disc_year</code></li>
</ul>
After removing the first few columns we obtain a preliminary look at our first data points. The format of these tables is drawn from <strong><a href="https://www.sonofacorner.com/beautiful-tables/" target="_blank" style="color: #573259;"> Beautiful Tables in Matplotlib, a Tutorial</a></strong> and <strong><a href="https://matplotlib.org/matplotblog/posts/how-to-create-custom-tables/?ref=sonofacorner.com" target="_blank" style="color: #573259;">How to create custom tables</a></strong>.

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-C.svg" target="_blank">
  <img src="/assets/img/2024-05-24-exoplanets-C.svg"  width="600" alt="Graph C" style="border: 2px solid #573259; border-radius: 3px;">
  </a>
</p>
<div style="height: 20px;"></div>

Exoplanet science is young in comparison to most other astronomical sciences. For one, we've known about exoplanets for little time the first exoplanet discovery being in 1992,

















<h3 align="center">Visualization and Analysis</h3>
<div style="height: 20px;"></div>
Next we will move to visualizing the data we have. Some processing has been omitted here and can be see nin my notebooks. 

I've opted for this project to use <code>matplotlib</code> and <code>seaborn</code>, tools which are sometimes portrayed as lacking in complexity or aesthetic quality. I wanted to develop graphs that still feel dynamic and engaging. I also employed lesser used matplotlib tools such as 3D graphs, tables and style sheets.

<h4>Discovery method</h4>
<div style="height: 20px;"></div>

<p> Examining the methods used to discover planets provides valuable insights into the underlying science. While exoplanets can be discovered in various ways, certain methods have been particularly successful. </p>

The discovery of planets in themselves is an interesting subject because exoplanets are fundamentally harder to detect than other celestial objects such as stars. Scientists have needed to develop complex indirect methods to track exoplanets. 

<ul>
  <li>The <strong>transit method</strong> exploits the shifts in light as a planet passes in front of its host star. The amount of dimming can be used to make conclusions about the planet's characeristics like mass, radiius and eve atmospheric conditions.</li>
  <li>The <strong>radial velocity method</strong> - tracking of gravity induced shifts in a star's position caused by the planet's gravitational pull. </li>
  <li>The <strong>microlensing</strong> technique tracks the bending of light as large exoplanets pass in front of a star.</li>
</ul>

All of these methods require a rich level of spectroscopic data to infer the composition or size of planets. As science progresses, the ability to gather and conduct analysis on this data improves. I have tracked the growth of the three most successful exoplanet discovery methods mentioned above, graphed since the start of the century.

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-J.svg" target="_blank">
  <img src="/assets/img/2024-05-24-exoplanets-J.svg" width="1000" alt="Graph J" style="border: 3px solid #573259; border-radius: 3px;">
  </a>
<div style="height: 20px;"></div>

<strong> Observations </strong>
<ul>
  <li>There's a clear increase in discoveries over the past decade.</li>
  <li>Some years, such as 2016, have seen enormous numbers of exoplanet discoveries. </li>
  <li>The <strong>transit method</strong> is clearly the most successful discovery method.</li>
</ul>

<p> Observational cosmologist Chris Impey mentioned in a recent <a href="https://www.youtube.com/watch?v=fbRfJTiQYtA&ab_channel=TheRoyalInstitution" target="_blank" style="color: #573259;">lecture</a> that the growth of discovery of the planets is comparable to the development of internet technology and telephones seen during the Dotcom bubble. So we can easily glean that exoplanet science is progressing at a very rapid pace.</p> 

<div style="height: 20px;"></div>
<h4>Distance from Earth</h4>
<div style="height: 20px;"></div>

We next track exoplanet distance from Earth; I've generated a histogram (lollipop plot) tracking exoplanets respective to their distance from Earth:

<div style="height: 20px;"></div>

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-D.svg" target="_blank">
  <img src="/assets/img/2024-05-24-exoplanets-D.svg" width="1200" height="600" alt="Graph D" style="border: 3px solid #573259; border-radius: 3px;">
  </a>
</p>
<div style="height: 20px;"></div>

<strong>Observation(s):</strong>
<ul>
  <li> More planets have been closer to Earth.</li>
  <li> Fairly consistent amount of planets found to 3500 light years away.</li>
  <li> Remarkably, a small number of planets have still been been accessed at 10,000 light years away. Recall, one light year is 9,461,000,000,000 km!</li>
</ul>

<p> We can also better evaluate the distribution of planets as a whole through a cumulative distribution function (CDF):</p>

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-E.svg" target="_blank">
  <img src="/assets/img/2024-05-24-exoplanets-E.svg" width="1200" height="600" alt="Graph E" style="border: 3px solid #573259; border-radius: 3px;">
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

<h4> Planet mass and radius </h4>

<div style="height: 20px;"></div>
Observing the disparity of planet size and mass can allow us to create a better demographic picture of the planets in our observed universe. Next we will take a look at exoplanet radius and mass. The following is a scatterplot of mass and radius. Note that the scatterplot colors are defined in terms of mass, and the mass is in a logarithmic scale.

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-F.svg" target="_blank">
  <img src="/assets/img/2024-05-24-exoplanets-F.svg" width="910.65" height="720" alt="Graph F" style="border: 3px solid #573259; border-radius: 3px;">
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















<h3 align="center"> Machine Learning Classification</h3>
<div style="height: 20px;"></div>

<p>But as NASA puts it, with radius and mass alone "we can see compositions ranging from rocky (like Earth and Venus) to gas-rich (like Jupiter and Saturn)". So those two features alone can play a signfiicant role in classifications, particularly since a planet's mass and size will correlate with its actual geologic properties. </p>

<p>Since this dataset does not provide existing classifications of planets, I will be performing unsupervised learning.</p>

<p>With this in mind I will be conducting a k-means algoirthm using the features of exoplanet radius and exoplanet mass alone using <code>scikit-learn</code>.</p>

<div style="height: 20px;"></div>
<h4> Silhouette score calculation</h4>
<div style="height: 20px;"></div>

<p> To determine the number of categories (clusters) we want to develop, we will use the <strong>silhouette score</strong> method. You may consult my script for a better understanding of this method. High scores indicate good clustering results, bad scores indicate bad results.</p>

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-G.svg" target="_blank">
  <img src="/assets/img/2024-05-24-exoplanets-G.svg" width="800" alt="Graph G" style="border: 3px solid #573259; border-radius: 3px;">
  </a>
</p>
<div style="height: 20px;"></div>

<strong>Observation(s): </strong>
<ul>
<li>Clearly the most favorable score is 4, so we will divide our exoplanet data into four categories in attempts to match them with existing categories.</li>
</ul>
<div style="height: 20px;"></div>

<div style="height: 20px;"></div>
<h4> Apply k-means clustering</h4>
<div style="height: 20px;"></div>

We run a kmeans model using scikit learn and add the clusters it has generate to our data. In the following graph we display those results, with each cluster differing in color and 

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-H.svg" target="_blank">
  <img src="/assets/img/2024-05-24-exoplanets-H.svg" width="910.65" height="720" alt="Graph H" style="border: 3px solid #573259; border-radius: 3px;">
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
  <img src="/assets/img/2024-05-24-exoplanets-I.png" width="700" alt="Graph I" style="border: 3px solid #573259; border-radius: 3px;">
  </a>
</p>
<div style="height: 20px;"></div>

<strong>Observation(s):</strong>
<ul>
<li>There doesn't appear to be a strong correlation between mass/radius and distance. This makes sense - we wouldn't expect composition and components of the planets to depend on their distance from Earth</li>
</ul>

<div style="height: 20px;"></div>

<h4>Cluster Analysis</h4>
<div style="height: 20px;"></div>

Let's take a look at how our clusters compared to existing exoplanet groupings.
<div style="height: 20px;"></div>

<h4 align="center">Cluster 1: Super-Earths, Sub-Neptunes and Super-Neptunes</h4>
<div class="container-table">
    <div class="half-table">
        <a href="/assets/img/2024-05-24-exoplanets-H1.svg" target="_blank"><img src="/assets/img/2024-05-24-exoplanets-H1.svg" width="500" style="border: 3px solid #573259; border-radius: 3px;"></a>
    </div>
    <div class="half-table">
        <!-- Content for the left half goes here -->
      <p>This cluster appears to host planets from at least two groups</p>
        <ul>
          <li><strong>Super-Earths</strong>, strictly defined as twice the size of Earth and up to 10 times its mass.</li>
          <li><strong>Sub-Neptune></strong>, planets smaller than Neptune's radius (3.88 Earth radius) even though it may have a larger mass. </li>
            <li><strong>Super-Neptune</strong> includes planets around 2 to 6 Earth radii and 10 to 50 Earth masses.</li>
        </ul>
    </div>
</div>
<p>This appears to be an extremely large collection of exoplanets, particularly with respect to mass from 0.2 to 1000 times Earth's mass. They include most planets under 10 radius, but that includes a wide-range of planets.</p>

<div style="height: 20px;"></div>

<h4 align="center">Cluster 2: Super-Neptunes/Super-Jupiters</h4>
<div class="container-table">
    <div class="half-table">
        <a href="/assets/img/2024-05-24-exoplanets-H2.svg" target="_blank"><img src="/assets/img/2024-05-24-exoplanets-H2.svg" width="500" style="border: 3px solid #573259; border-radius: 3px;"></a>
    </div>
    <div class="half-table">
        <!-- Content for the right half goes here -->
       <p>Most probably these contain:</p>
       <ul>
         <li><strong>Super Jupiters</strong>: A Super-Jupiter has a mass exceeding that of Jupiter, typically greater than 1.5 times Jupiter's mass (approximately 1.9 x 10^27 kg).</li>
         <li><strong>Super Neptunes</strong>: A Super-Neptune has mass ranging from about 10 to 20 times that of Earth, larger than Neptune but less massive than Super Jupiters.</li>
       </ul>   
    </div>
</div>

These appear to contain a large amount of relatively moderately-sized planets. A large number of planets can be found in this categorization.
<div style="height: 20px;"></div>

<h4 align="center">Cluster 3: Super-Jupiters and Brown-Dwarfs </h4>
<div class="container-table">
    <div class="half-table">
        <!-- Image for the right half goes here -->
        <a href="/assets/img/2024-05-24-exoplanets-H3.svg" target="_blank"><img src="/assets/img/2024-05-24-exoplanets-H3.svg" width="500" style="border: 3px solid #573259; border-radius: 3px;"></a>
    </div>
  <div class="half-table">
        <!-- Content for the left half goes here -->
       <p>This cluster contains exoplanets with enormous masses and radii. These could be:</p>
    <ul>
      <li><strong>Super-Jupiters</strong> given their clear superior size to Jupiter.</li>
      <li> <strong>Brown-Dwarfs</strong>, whose tend to have a mass-to-Earth ratio between 4,000 to 25,000. In fact, the the entire cluster falls in this regime.</li>
    </ul>
    </div>
</div>
<div style="height: 20px;"></div>

<p>Brown dwarfs are particularly fascinating because they lie in an area of classification between planets and stars by exhibiting properties of both. Usually brown dwarfs are born like stars, large enough to collapse under their own gravity, but usually fail to become large enough to sustain the nuclear fusion of hydrogen. For this reason they're often a hotly debated topic in terms of classification.</p>

<div style="height: 20px;"></div>

<h4 align="center">Cluster 4: Super-Jupiters</h4>

<div class="container-table">
    <div class="half-table">
        <!-- Image for the left half goes here -->
        <a href="/assets/img/2024-05-24-exoplanets-H4.svg" target="_blank"><img src="/assets/img/2024-05-24-exoplanets-H4.svg" width="500" style="border: 3px solid #573259; border-radius: 3px;"></a>
    </div>
    <div class="half-table">
        <!-- Content for the right half goes here -->
        <p>Most likely these are: </p>
      <ul>
        <li><strong>Super-Jupiters</strong> given their clear superior size to Jupiter.</li>
      </ul>
    </div>
</div>
<div style="height: 20px;"></div>

<p>I would also take note the amount of range in this cluster, like cluster three. I would be surprised to expect Planets with radii of 3 and 35 to share many inherent qualities. This also appears to fall into the Super-Jupiter category. The maximum just touches on the Brown-Dwarf category. So these may be considered as Super-Jupiters.</p>

<h3>Reflection and final observations</h3>
<p>A few points in </p>
<ul>
  <li><strong>Some relevant patterns, no neat classifications</strong> - Aside from the very-large brown dwarfs, no cluster created distinct categories.</li>
  <li><strong>Clustering could have benefited from more features</strong> - Adding features exoplanet orbitals or host star tempearture and size could have siphoned off clearer groups</li>
  <li><strong>Clustering is difficult for badly defined categories</strong> - There are some quntitative metrics for planet categories, but most exoplanets do not fit into neat categories.</li>
</ul>

<h4>Questioning our existing categorizations</h4>
<div style="height: 20px;"></div>

<p>Expanding on this last point, there's criticism to be had on the way in which we base planet categorizations on known objects in our Solar System. This in essence a type of heliocentrism; we group planets in the Universe based on historical observations and our local knowledge of the Universe, not fundamental qualities possessed by planets. In this sense, there a fundamental astronomical difference between a Sub-Neptune and a Super-Neptune aside from it's relative size to Neptune? There is reason to take issues with these groups on scientific grounds. </p>


Super-Earths or Sub-Neptunes, we base planets on our
Classifying a planet based on the ones in our Solar System doesn't lead to categories that actually reflect statistical trends in the universe. Our current categorizations don' take into account the vast diversity in planets that exceed Jupiter's size. On a level deeper, the idea of what a planet even is is a political matter among astronomists.


<h3 align="center">Resources</h3>
<div style="height: 20px;"></div>

If worked has piqued your interest on exoplanet science, here are a few resources I appreciate to keep reading on the matter:
<ul>
<li><a href="https://www.quantamagazine.org/the-best-neighborhoods-for-starting-a-life-in-the-galaxy-20240124/" target="_blank" style="color: #573259;">The Best Neighborhoods for Starting a Life in the Galaxy</a></li>
</ul>

