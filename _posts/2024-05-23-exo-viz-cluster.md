---
layout: post
title: Worlds Beyond - Visualizing and Clustering NASA Exoplanet Data
date: 2024-05-23
tags: [projects, tech, data science, visualization, machine learning, kmeans]
image: 2024-05-24-exoplanets-cover.png
github: https://github.com/ucheetah/exoplanet-viz-cluster
custom_excerpt: Peering into exoplanet science through the lens of data. I grab a dataset from the NASA exoplanets archive on which I'll explore, visualize and perform unsupervised learning through a k-means clustering algorithm.
---

<p align="center"> <em> This post and projects marks the beginning of my new personal website. I'll be sharing ideas and creations in data science that drive me. My first project was created purely out my curiosity for astronomical science and to, as always, hone my skills as a data scientist.</em> </p>

<hr>
<div style="height: 20px;"></div>

<p>This project peers into exoplanet science through the lens of data. I grab a dataset from the <strong><a href="https://exoplanetarchive.ipac.caltech.edu/index.html" target="_blank">NASA exoplanets archive</a></strong> on which I'll explore, visualize and perform unsupervised learning through a k-means clustering algorithm.</p>

<h3 align="center"> Project summary</h3>
<div style="height: 20px;"></div>

<table align="center">
  <tr>
    <td align="center" width=150><strong> Project Goals </strong> </td>
    <td>
    Query and clean data from NASA exoplanet archive; perform exploratory data analysis and visualize planet features; and employ k-means clustering comparing groups to existing exoplanet classifications.
    </td>
  </tr>
  <tr>
    <td width=150><strong>Languages</strong></td>
    <td>Python (pandas, numpy, matplotlib, seaborn, scikit-learn), SQL (one humble query)</td>
  </tr>
  <tr>
    <td width=150><strong>Techniques and skills</strong></td>
    <td>Data querying, data cleaning, missing value detection/outlier handling, visualization, machine learning, unsupervised learning, k-means clustering, communicating scientific insights, </td>
  </tr>
  <tr>
    <td width=150><strong>Project GitHub</strong></td>
    <td> <strong><a href="https://github.com/ucheetah/exoplanet-viz-cluster/tree/main" target="_blank">GitHub Repo - exoplanet-viz-cluster</a></strong> </td>
  </tr>
</table>

<div style="height: 20px;"></div>
<h3 align="center"> Background</h3>
<div style="height: 20px;"></div>

<p>Our popular conception of space is dominated by stars. Understandably though. Not to get poetic, but for as long there's been life on Earth, we've gazed at the night sky. Planets, on the other hand, just don't get the same notoriety. The nine we know well certainly do. But the planets outside our Solar System, extrasolar planets, or more commonly <strong>exoplanets</strong>, tend not to</p>

<p>I would argue that this view is changing though. Most astronomers recognize something invaluable that exoplanet offers offers - confirming the existence of extraterrestrial life. Many physicists claim that we're guaranteed to find life beyond our Solar System given enough searching and the genuine unlikelihood that life on Earth is unique.</p>

<p>This project investigates the existing data on exoplanets. I'll <strong>perform exploratory data analysis</strong> on an exoplanet dataset provided by NASA. I will then <strong>visualize</strong> this data with the goal of rendering graphs that are dynamic and give big picture takeaways. Then, I'll <strong>conduct unsupervised learning</strong> - a k-means clustering algorithm - on the data and compare it to existing exoplanet classifications.</p>

<div style="height: 20px;"></div>

<h3 align="center">Data collection and cleaning</h3> 
<div style="height: 20px;"></div>

<p>I will work with the <em><a href="https://exoplanetarchive.ipac.caltech.edu/docs/API_PS_columns.html" target="_blank"> Planetary Systems</a></em> dataset from the NASA exoplanets archive, a collaboration between Caltech and NASA under its Exoplanet Exploration Program. The dataset contains records of all 5638 confirmed exoplanets.</p>

<figure style="text-align: center;">
   <a href = "https://exoplanetarchive.ipac.caltech.edu/index.html" target="_blank">
      <img src="/assets/img/nasa_exoplanet_homepage.png" width = "550" alt="NASA Homepage">
    </a>
      <figcaption>NASA Exoplanet Archive Website</figcaption> 
</figure>

You may consult my notebook for the full process. I've queried data from NASA's <strong>Table Access Protocol (TAP)</strong> using SQL and Python's <strong><code>requests</code></strong> package. I extracted <strong>planet radius</strong>, <strong>planet mass</strong>, <strong>distance from Earth</strong>, <strong>discovery method</strong> and <strong>discovery year</strong> as parameters of interesst. After some basic pre-processing here's an unvarnished look at our raw data:

<div style="height: 10px;"></div>
<figure style="text-align: center;">
  <a href="/assets/img/exoplanet-raw-table.png" target="_blank">
  <img src="/assets/img/exoplanet-raw-table.png"  width="600" alt="Exoplanet raw dataset">
  </a>
  <figcaption> <em>Planetary Systems</em> dataset shown in interactive notebook. </figcaption> 
</figure>
<div style="height: 10px;"></div>

Here's a preliminary summary of this data. The table includes counts of each feature (counts below 5638 indicate missing data), minimum and maximum values, and  25th, 50th and 75th percentiles: 

<div style="height: 10px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-C.svg" target="_blank">
  <img src="/assets/img/2024-05-24-exoplanets-C.svg"  width="600" alt="Graph C">
  </a>
</p>
<div style="height: 10px;"></div>

<strong>Observation(s):</strong>
<ul>
  <li>Exoplanet science is young - the first exoplanet was found in 1992. Interestingly though, only an exoplanet discovery three years later would snatch a <a href="https://www.insidescience.org/news/all-exoplanets-came-1995">Nobel Prize (it's always political)</a>
</li>
  <li>There is a surreal amount of diversity in planet characteristics - planets from 4 to 25k light years, 0.4 to 10k times Earth's mass and 0.3 to 77.3 times Earth's radius.</li>
</ul>
With this first glimpse let's dive in deeper.













<div style="height: 20px;"></div>
<h3 align="center">Visualization and Analysis</h3>
<div style="height: 20px;"></div>
Next I'll visualize the data. I'll only be showing outputs. For pre-processing details and graph construction consult my notebook and/or scripts. 

I've used <strong><code>matplotlib</code></strong> and <strong><code>seaborn</code></strong>. I wanted to challenge the notion that these packages lack in complexity or aesthetic quality by developing graphs that still feel dynamic and engaging, and employing lesser-used tools such as 3D graphs, matplotlib tables and stylesheets.

<div style="height: 20px;"></div>
<h4>Discovery method</h4>
<div style="height: 20px;"></div>

<p>The discovery of exoplanets in itself is interesting. Since exoplanets are fundamentally harder to detect than other bodies like stars, finding them has required the development of complex methods using fine-grained spectroscopic data:</p>

<ul>
  <li>The <strong>transit method</strong> tracks the shift in light as an exoplanet crosses a visible star, creating a "mini-eclipse" with the Earth.</li>
  <li>The <strong>radial velocity method</strong> tracks shifts in a star's motion due to the gravitational pull of an orbiting exoplanet.</li>
  <li>The <strong>microlensing</strong> method tracks distorsions of light as large exoplanets pass in front of a star.</li>
</ul>

<p>I've tracked the evolution in exoplanet discoveries since the first in 1992. Along the lines of observational cosmologist <strong><a href="https://www.youtube.com/watch?v=fbRfJTiQYtA&ab_channel=TheRoyalInstitution" target="_blank">Chris Impey</a></strong>'s perspective, the rapid discovery of exoplanets is comparable to the explosive growth of internet technology during the Dotcom bubble, and that growth is on full display:</p> 

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-J.svg" target="_blank">
  <img src="/assets/img/2024-05-24-exoplanets-J.svg" width="850" alt="Graph J" style="border: 3px solid #573259; border-radius: 3px;">
  </a>
<div style="height: 20px;"></div>

<strong> Observations </strong>
<ul>
  <li>There's a clear increase in discoveries over the past decade.</li>
  <li>Some years, such as 2016, have seen enormous numbers of exoplanet discoveries. </li>
  <li>The transit method is clearly the most successful discovery method.</li>
</ul>

<div style="height: 20px;"></div>
<h4>Distance from Earth</h4>
<div style="height: 20px;"></div>

Next I look at exoplanet distance from Earth; I've generated a histogram - technically a lollipop plot - tracking this metric:

<div style="height: 20px;"></div>

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-D.svg" target="_blank">
  <img src="/assets/img/2024-05-24-exoplanets-D.svg" width="850" alt="Graph D" style="border: 3px solid #573259; border-radius: 3px;">
  </a>
</p>
<div style="height: 20px;"></div>

<strong>Observation(s):</strong>
<ul>
  <li> Clear decrease in exoplanets as the distance increases. Unsurprising and likely due to observational limits.</li>
  <li> Highest concentration of planets within the first 2000 light years.</li>
  <li> Planets drop significantly around 4000 light years. Very few exoplanets beyond 6000 years.</li>
  <li> Remarkably a small number of planets are accessed beyond 8,000 light years (Recall that one light year is 9,461,000,000,000 km)</li>
</ul>

<p> We can also better evaluate the distribution of planets as a whole through a cumulative distribution function:</p>

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-E.svg" target="_blank">
  <img src="/assets/img/2024-05-24-exoplanets-E.svg" width="850" alt="Graph E" style="border: 3px solid #573259; border-radius: 3px;">
  </a>
</p>
<div style="height: 20px;"></div>

<strong>Observation(s):</strong>
<ul>
  <li> Close to 90% of known exoplanets are within the ten thousand light year range. </li>
  <li> The growth slows down as we move beyond 4000 light years,. </li>
</ul>
<div style="height: 20px;"></div>

<h4> Planet mass and radius </h4>

<div style="height: 20px;"></div>
Now I'll track radius and mass. This scatterplot compares mass and radius. Mass is also describe with hue.

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-F.svg" target="_blank">
  <img src="/assets/img/2024-05-24-exoplanets-F.svg" width="850" alt="Graph F" style="border: 3px solid #573259; border-radius: 3px;">
  </a>
</p>
<div style="height: 20px;"></div>

<strong>Observation(s):</strong>

<ul>
<li> Apparent linear relationship between log of planet mass and planet radius.</li>
<li> Appears to be two clumps of data, one with smaller radius and mass, another with larger.</li>
<li> Some outliers with very large mass and radii.</li>
</ul>

However planets differ greatly in their compositions and densities so we would expect there to be quite a bit of fluctuation. 

<div style="height: 20px;"></div>















<h3 align="center"> Machine Learning Classification</h3>
<div style="height: 20px;"></div>

<p>To better assess the influx of planets we're finding, a number of different categorizations exist for ... With radius and mass alone "we can see compositions ranging from rocky (like Earth and Venus) to gas-rich (like Jupiter and Saturn)".</p>

<p>We group data based on their features in to a number of categories. We call it unsupervised because have no prior data on how to categorize the data. This method is most powerful when we do not have existing data to train the model on how to classify</p>

<p>I Since this dataset does not provide existing classifications of planets, I will be performing unsupervised learning.</p>

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
  <img src="/assets/img/2024-05-24-exoplanets-H.svg" width="850" alt="Graph H" style="border: 3px solid #573259; border-radius: 3px;">
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
<li>
  
</li>
</ul>

<ul>
  <li>
    The format of these tables is drawn from <strong><a href="https://www.sonofacorner.com/beautiful-tables/" target="_blank"> Beautiful Tables in Matplotlib, a Tutorial</a></strong> and <strong><a href="https://matplotlib.org/matplotblog/posts/how-to-create-custom-tables/?ref=sonofacorner.com" target="_blank"> How to create custom tables</a></strong>.
  </li>
<li>
<strong><a href="https://matplotlib.org/stable/users/explain/customizing.html" target="_blank">Customizing Matplotlib with style sheets and rcParams</a></strong> - Sometimes direct from the source is best. Great demo on customizing sheets and understanding the underlying rcParams function in Matplotlib, which I used to make my own style sheet.
</li>
<li>
<strong><a href="https://python-graph-gallery.com/lollipop-plot/">Lollipop chart | Python & Matplotlib examples | The Python Graph Gallery</a></strong> - Short intro to lollipop plots. This was my first time using one; usually they're used for barplots (categorical data) but they can also be appropriate for histograms given the context. Great to avoid <a href="https://en.wikipedia.org/wiki/Moir%C3%A9_pattern">Moiré patterns</a>
</li>
<li>
<strong><a href="https://jekyllrb.com/docs/github-pages/">GitHub Pages</a></strong> - Glad I made my website using GitHub Pages. It's a great middle-ground between full out website creation from scratch and simpler options with little room for personalization.
</li>
</ul>





