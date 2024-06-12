---
layout: post
title: Worlds Beyond - Visualizing and Clustering NASA Exoplanet Data
date: 2024-06-11
tags: [projects, tech, data science, visualization, machine learning, kmeans]
image: 2024-05-24-exoplanets-cover.png
github: https://github.com/ucheetah/exoplanet-viz-cluster
custom_excerpt: Peering into exoplanet science through the lens of data. I grab a dataset from the NASA exoplanets archive on which I'll explore, visualize and perform unsupervised learning through a <math xmlns="http://www.w3.org/1998/Math/MathML"><mi>k</mi></math>-means clustering algorithm.
est_time: 10 min read
---

<p align="center"> <em> This project marks the beginning of my new personal website. Learning is eternal: hoping to document some of that journey here. My first project was inspired out of my curiosity for astronomy and the perpetual goal of honing my skills in data science.</em> </p>

<hr>
<div style="height: 20px;"></div>

<p>This project peers into exoplanet science through the lens of data. I grab a dataset from the <strong><a href="https://exoplanetarchive.ipac.caltech.edu/index.html" target="blank">NASA exoplanets archive</a></strong> on which I'll explore, visualize and perform unsupervised learning through a <math xmlns="http://www.w3.org/1998/Math/MathML"><mi>k</mi></math>-means clustering algorithm.</p>

<h3 align="center"> Project summary</h3>
<div style="height: 20px;"></div>

<table align="center">
  <tr>
    <td align="center" width=150><strong> Project Goals </strong> </td>
    <td>
    Query and clean data from NASA exoplanet archive; perform exploratory data analysis and visualize planet features; and employ <math xmlns="http://www.w3.org/1998/Math/MathML"><mi>k</mi></math>-means clustering comparing groups to existing exoplanet classifications.
    </td>
  </tr>
  <tr>
    <td width=150><strong>Languages</strong></td>
    <td>Python (pandas, numpy, matplotlib, seaborn, scikit-learn), SQL (one humble query)</td>
  </tr>
  <tr>
    <td width=150><strong>Techniques and skills</strong></td>
    <td>Data querying, data cleaning, missing value detection, outlier handling, visualization, machine learning, unsupervised learning, <math xmlns="http://www.w3.org/1998/Math/MathML"><mi>k</mi></math>-means clustering </td>
  </tr>
  <tr>
    <td width=150><strong>Project GitHub</strong></td>
    <td> <strong><a href="https://github.com/ucheetah/exoplanet-viz-cluster/tree/main" target="blank">GitHub Repo - exoplanet-viz-cluster</a></strong> </td>
  </tr>
</table>








<div style="height: 20px;"></div>
<hr>
<div style="height: 20px;"></div>
<h3 align="center"> Background</h3>
<div style="height: 20px;"></div>

<p>Our popular conception of space is dominated by stars. Understandably though. Not to get poetic, but for as long there's been life on Earth, we've gazed at the night sky. Planets, on the other hand, just don't get the same notoriety. While we're accustomed to the nine in our Solar System (well, eight since the <strong><a href="https://neildegrassetyson.com/essays/2007-06-plutos-requiem/" target="blank">Pluto debacle</a></strong>), <strong>exoplanets</strong> - planets outside our Solar System - don't enjoy the same level of recognition</p>

<p>I'd argue that this view is changing, though. Most astronomers recognize an invaluable prospect that exoplanet science offers - confirming the existence of extraterrestrial life. In fact physicists claim that we're guaranteed to find life beyond our Solar System given enough searching and the genuine unlikelihood that life on Earth is unique.</p>

<p>This project investigates the existing record on exoplanets. I'll perform <strong>exploratory data analysis</strong> on exoplanet data provided by NASA. I will then <strong>visualize</strong> this data with the goal of rendering big picture takeaways. Lastly I'll conduct <strong>unsupervised machine learning</strong> using a <math xmlns="http://www.w3.org/1998/Math/MathML"><mi>k</mi></math>-means clustering algorithm on the data and compare it to existing exoplanet classifications. For the full technical process consult my GitHub repository.</p>












<div style="height: 20px;"></div>
<hr>
<div style="height: 20px;"></div>
<h3 align="center">Data collection and cleaning</h3> 
<div style="height: 20px;"></div>

<p>I will work with the <em><a href="https://exoplanetarchive.ipac.caltech.edu/docs/API_PS_columns.html" target="blank"> Planetary Systems</a></em> dataset, which contains records of all 5638 confirmed exoplanets, provided by the NASA exoplanets archive. The archive is a collaboration between Caltech and NASA under its Exoplanet Exploration Program.</p>

<figure style="text-align: center;">
   <a href = "https://exoplanetarchive.ipac.caltech.edu/index.html" target="blank">
      <img src="/assets/img/nasa_exoplanet_homepage.png" width = "400" alt="NASA Homepage">
    </a>
      <figcaption>NASA Exoplanet Archive Website</figcaption> 
</figure>

I've grabbed <strong>planet radius</strong>, <strong>planet mass</strong>, <strong>distance from Earth</strong>, <strong>discovery method</strong> and <strong>discovery year</strong> as parameters of interest. 

I've queried this data from NASA's <strong>Table Access Protocol (TAP)</strong> using SQL and Python's <strong><code>requests</code></strong> package and conducted some basic pre-processing. A quick peek at the data gives us:

<figure style="text-align: center;">
  <a href="/assets/img/exoplanet-raw-table.png" target="blank">
  <img src="/assets/img/exoplanet-raw-table.png"  width="600" alt="Exoplanet raw dataset">
  </a>
  <figcaption> <em>Planetary Systems</em> dataset shown in interactive notebook. </figcaption> 
</figure>

Here's a more comprehensive summary of the data, including <strong>counts</strong> of each feature (counts below 5638 indicate missing data), <strong>minimum</strong> and <strong>maximum values</strong>, and  <strong>25th</strong>, <strong>50th</strong> and <strong>75th percentiles</strong>: 

<div style="height: 10px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-C.svg" target="blank">
  <img src="/assets/img/2024-05-24-exoplanets-C.svg"  width="600" alt="Graph C">
  </a>
</p>
<div style="height: 10px;"></div>

<strong>Observation(s):</strong>
<ul>
  <li>Exoplanet science is young - the first exoplanet was found in 1992. Interestingly though, only the researchers behind an exoplanet discovery three years later would snatch a <strong><a href="https://www.insidescience.org/news/all-exoplanets-came-1995" target="blank">Nobel Prize</a></strong> (it's always political).
</li>
  <li>There is a surreal amount of diversity in planet characteristics - these range from
  <ul>
    <li>4 to 25,000 light years away</li>
    <li>0.4 to 10,000 times Earth's mass (M&oplus;)</li>
    <li>0.3 to 77.3 times Earth's radius (R&oplus;)</li>
  </ul>
  </li>
</ul>
With this first glimpse let's dive in deeper with visualizations.










<div style="height: 20px;"></div>

<hr>

<div style="height: 20px;"></div>
<h3 align="center">Visualization and Analysis</h3>
<div style="height: 20px;"></div>
Next I'll visualize the data. I'll only be showing outputs created using <strong><code>matplotlib</code></strong> and <strong><code>seaborn</code></strong>. I wanted to challenge the notion that these packages lack in complexity or aesthetic quality by developing graphs that still feel dynamic, and employing lesser-used tools such as 3D graphs, matplotlib tables and stylesheets.

<div style="height: 20px;"></div>
<h4>Discovery method</h4>
<div style="height: 20px;"></div>

<p>Since exoplanets are fundamentally harder to detect than other bodies like stars, finding them has required the development of complex methods using fine-grained spectroscopic data:</p>

<ul>
  <li>The <strong>transit method</strong> tracks the shift in light as an exoplanet crosses a visible star, creating a "mini-eclipse" with the Earth.</li>
  <li>The <strong>radial velocity method</strong> tracks shifts in a star's motion due to the gravitational pull of an orbiting exoplanet.</li>
  <li>The <strong>microlensing</strong> method tracks distortions of light as large exoplanets pass in front of a star.</li>
</ul>

<p>I've tracked the evolution in exoplanet discoveries since the first in 1992:</p>

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-J.svg" target="blank">
  <img src="/assets/img/2024-05-24-exoplanets-J.svg" width="850" alt="Graph J" style="border: 3px solid #573259; border-radius: 3px;">
  </a>
<div style="height: 20px;"></div>


<strong> Observations </strong>
<ul>
  <li>Obvious uptick in findings over the past decade.</li>
  <li>2016 saw a near-doubling of exoplanets, owed to the <strong><a href="https://www.jpl.nasa.gov/missions/kepler" target="blank">Kepler Space Telescope Mission</a></strong>, the most successful exoplanet mission to date.</li>
  <li>Transit method is clearly most successful.</li>
</ul>
<div style="height: 10px;"></div>
<p>Observational cosmologist <strong><a href="https://www.youtube.com/watch?v=fbRfJTiQYtA&ab_channel=TheRoyalInstitution" target="blank">Chris Impey</a></strong> observed that the rapid discovery of exoplanets is comparable to the explosive growth of internet technology during the Dotcom bubble. Above that growth is on full display.</p> 

<div style="height: 20px;"></div>
<h4>Distance from Earth</h4>
<div style="height: 20px;"></div>

Next I track exoplanet distance from Earth:

<div style="height: 20px;"></div>

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-D.svg" target="blank">
  <img src="/assets/img/2024-05-24-exoplanets-D.svg" width="850" alt="Graph D" style="border: 3px solid #573259; border-radius: 3px;">
  </a>
</p>
<div style="height: 20px;"></div>

<strong>Observation(s):</strong>
<ul>
  <li> Clear decrease in exoplanet findings with distance. Unsurprising and likely due to observational limits.</li>
  <li> Highest concentration within the first 2,000 light years.</li>
  <li> Very few exoplanets beyond 6,000 light years, but remarkably a small number of planets accessed beyond 8,000 light years.</li>
</ul>

<p> I'll also evaluate the planet distribution using a cumulative distribution function, which indicates to us the probability (<math xmlns="http://www.w3.org/1998/Math/MathML"><mi>y</mi></math>) that an exoplanet will be found at a distance less than or equal to some value (<math xmlns="http://www.w3.org/1998/Math/MathML"><mi>x</mi></math>) from Earth:</p>

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-E.svg" target="blank">
  <img src="/assets/img/2024-05-24-exoplanets-E.svg" width="850" alt="Graph E" style="border: 3px solid #573259; border-radius: 3px;">
  </a>
</p>
<div style="height: 20px;"></div>

<strong>Observation(s):</strong>
<ul>
  <li> For example, about 60% of known planets can be found within 2,000 light years from Earth.</li>
  <li> About 90% of exoplanets exist within the 10,000 light year range. </li>
  <li> Growth slows down past 4,000 light years. </li>
</ul>
<div style="height: 20px;"></div>

<h4> Planet mass and radius </h4>

<div style="height: 20px;"></div>
Now I'll track radius and mass with a scatterplot, where each dot is an exoplanet. Note that mass (<math xmlns="http://www.w3.org/1998/Math/MathML"><mi>y</mi></math>) is on a logarithmic scale and also described with color:

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-F.svg" target="blank">
  <img src="/assets/img/2024-05-24-exoplanets-F.svg" width="850" alt="Graph F" style="border: 3px solid #573259; border-radius: 3px;">
  </a>
</p>
<div style="height: 20px;"></div>

<strong>Observation(s):</strong>

<ul>
<li> Apparent linear relationship between <math xmlns="http://www.w3.org/1998/Math/MathML"><mi>log</mi></math> of planet mass and planet radius.</li>
<li> Two major groups: smaller vs. moderate radius and mass</li>
<li> A few outliers with very large mass and radii.</li>
</ul>
<br>
These graphs have given us a fairly good sense of the ensemble based on what I've grabbed. The exploration could be endless of course, but I'll now shift to the machine learning portion. 









<div style="height: 20px;"></div>
<hr>
<div style="height: 20px;"></div>



<h3 align="center"> Machine Learning Classification</h3>
<div style="height: 20px;"></div>
<p>With some greater insight under our belt, I'll apply a machine learning model to our data, categorizing them into distinct groups using clustering.</p>
  
<p>Various categorizations have been created by astronomers since the first exoplanet discovery in 1992. When considering size, for instance, we often hear of these widely-used classifications:</p>

<ul>
  <li><strong>Super-Earths</strong> (1.0–1.75 R&oplus;) - Planets larger than Earth</li>
  <li><strong>Sub-Neptunes</strong> (1.75–3.5 R&oplus;) - Planets smaller than Neptune</li>
  <li><strong>Sub-Jovians</strong> (3.5–6.0 R&oplus;) - Planets smaller than Jupiter</li>
  <li><strong>Jovians</strong> (6–14.3 R&oplus;) - Planets near Jupiter's size</li>
</ul>

<p>For my machine learning applications, I aim to replicate these classifications using our exoplanet data. This involves <strong>unsupervised learning</strong>, where the model doesn't rely on labeled training data for the classifications we're trying to create. 
  
<p>I'll employ a <strong><math xmlns="http://www.w3.org/1998/Math/MathML"><mstyle mathvariant="bold"><mi>k</mi></mstyle></math>-means clustering</strong> algorithm, which for any number <math xmlns="http://www.w3.org/1998/Math/MathML"><mi>k</mi></math> groups the data based on select features - in this case, mass and radius - and returns <math xmlns="http://www.w3.org/1998/Math/MathML"><mi>k</mi></math> clusters based on their similarities. I'll do this using <strong><code>scikit-learn</code></strong>. I won't touch on the complexities of the algorithm itself here.</p>

<div style="height: 20px;"></div>
<h4> Silhouette Score calculation</h4>
<div style="height: 20px;"></div>

<p>I'll use the <strong>Silhouette Score Method</strong> to determine the number of categories (clusters) that's best to model. This method tests the algorithm on different groupings (one group, two groups,...) and attributes to each a score based on how well the algorithm will generate members that are alike. High scores indicate good grouping results, low scores indicate ineffective groupings.</p>

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-G.svg" target="blank">
  <img src="/assets/img/2024-05-24-exoplanets-G.svg" width="550" alt="Graph G" style="border: 3px solid #573259; border-radius: 3px;">
  </a>
</p>
<div style="height: 20px;"></div>

<strong>Observation(s): </strong>
<ul>
<li>Clearly, the most favorable score is four.</li>
</ul>

<div style="height: 20px;"></div>
<h4> Apply <math xmlns="http://www.w3.org/1998/Math/MathML"><mi>k</mi></math>-means clustering algorithm</h4>
<div style="height: 20px;"></div>

<p>Based on the Silhouette Method, I ran a <math xmlns="http://www.w3.org/1998/Math/MathML"><mi>k</mi></math>-means model on the exoplanet data generating four clusters.</p>

<p>Here's a scatterplot displaying the results. Colors represent different clusters, <strong>centroids (<code>+</code>)</strong> represent the mean values of each cluster.</p>

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-H.svg" target="blank">
  <img src="/assets/img/2024-05-24-exoplanets-H.svg" width="850" alt="Graph H" style="border: 3px solid #573259; border-radius: 3px;">
  </a>
</p>

<div style="height: 20px;"></div>
<strong>Observation(s):</strong>
<ul>
<li>Clusters 1 and 2 appear to be well categorized, representing planets with small radius/mass and medium radius/mass, respectively.</li>
<li>Clusters 3 and 4 are in the similar vicinity but span very wide ranges of radius, which could be suspect.</li>
</ul>
Adding distance as a third dimension:

<div style="height: 20px;"></div>
<p align="center">
  <a href="/assets/img/2024-05-24-exoplanets-I.png" target="blank">
  <img src="/assets/img/2024-05-24-exoplanets-I.png" width="700" alt="Graph I" style="border: 3px solid #573259; border-radius: 3px;">
  </a>
</p>
<div style="height: 20px;"></div>

<strong>Observation(s):</strong>
<ul>
<li>No strong correlation between mass/radius and distance. Expected - we wouldn't expect planet characteristics to depend on their distance from Earth.</li>
</ul>

<div style="height: 20px;"></div>

<h4>Cluster Analysis</h4>
<div style="height: 20px;"></div>

Let's look closer at each group and how each cluster fares to existing exoplanet groupings:
<div style="height: 20px;"></div>

<h5 align="center">Cluster 1</h5>
<strong><p align="center">Smaller planets</p></strong>
<div class="container-table">
    <div class="half-table">
        <a href="/assets/img/2024-05-24-exoplanets-H1.svg" target="blank"><img src="/assets/img/2024-05-24-exoplanets-H1.svg" width="500" style="border: 3px solid #573259; border-radius: 3px;"></a>
    </div>
    <div class="half-table">
        <!-- Content for the left half goes here -->
      <p>These likely contain:</p>
        <ul>
          <li><strong>Super-Earths</strong>, 1-10 times M&oplus;.</li>
          <li><strong>Sub-Neptune</strong>, less than 3.88 times R&oplus; (Neptune's radius). </li>
          <li><strong>Super-Neptune</strong> 2-6 times R&oplus; and 10-50 times M&oplus;.</li>
        </ul>
      <div style="height: 10px;"></div>
      Large cluster with enormous range in mass (0.2-1000 times M&oplus;).
    </div>
</div>

<div style="height: 20px;"></div>

<h5 align="center">Cluster 2</h5>
<strong><p align="center">Medium-sized planets</p></strong>
<div class="container-table">
    <div class="half-table">
        <a href="/assets/img/2024-05-24-exoplanets-H2.svg" target="blank"><img src="/assets/img/2024-05-24-exoplanets-H2.svg" width="500" style="border: 3px solid #573259; border-radius: 3px;"></a>
    </div>
    <div class="half-table">
        <!-- Content for the right half goes here -->
       <p>These likely contain:</p>
       <ul>
         <li><strong>Super Jupiters</strong>: over 318 times M&oplus; (1.5 times Jupiter mass).</li>
         <li><strong>Super Neptunes</strong>: 10-20 times M&oplus;.</li>
         <li><strong>Jovians</strong> 6–14.3 times R&oplus;</li>
       </ul>   
      <div style="height: 10px;"></div>
       <p>This is the largest cluster. It's also the cluster with the smallest spread of values. Relatively few outliers.</p>
    </div>
</div>

<div style="height: 20px;"></div>

<h5 align="center">Cluster 3</h5>
<strong><p align="center">Large and massive planets</p></strong>
<div class="container-table">
    <div class="half-table">
        <!-- Image for the right half goes here -->
        <a href="/assets/img/2024-05-24-exoplanets-H3.svg" target="blank"><img src="/assets/img/2024-05-24-exoplanets-H3.svg" width="500" style="border: 3px solid #573259; border-radius: 3px;"></a>
    </div>
  <div class="half-table">
        <!-- Content for the left half goes here -->
    <p>These likely contain:</p>
    <ul>
      <li><strong>Super-Jupiters</strong> given their clear superior size to Jupiter.</li>
      <li> <strong>Brown-Dwarfs</strong>, whose tend to have a mass-to-Earth ratio between 4,000 to 25,000. In fact, the the entire cluster falls in this regime.</li>
    </ul>
    <div style="height: 10px;"></div>
    The model appears to have identified all the brown-dwarfs.
    </div>
</div>
<div style="height: 20px;"></div>
 
<p><strong>Brown dwarfs</strong> are  fascinating because they lie in an area of classification between planets and stars, exhibiting properties of both. Usually they're born like stars, but fail to sustain the nuclear fusion of hydrogen; whether they're 'proper' planets is a matter of debate.</p>

<div style="height: 20px;"></div>

<h5 align="center">Cluster 4</h5>
<strong><p align="center">Also large and massive planets</p></strong>
<div class="container-table">
    <div class="half-table">
        <!-- Image for the left half goes here -->
        <a href="/assets/img/2024-05-24-exoplanets-H4.svg" target="blank"><img src="/assets/img/2024-05-24-exoplanets-H4.svg" width="500" style="border: 3px solid #573259; border-radius: 3px;"></a>
    </div>
    <div class="half-table">
        <!-- Content for the right half goes here -->
        <p>Most likely these contain: </p>
      <ul>
        <li><strong>Super-Jupiters</strong> given their clear superior size to Jupiter.</li>
      </ul>
      <div style="height: 10px;"></div>
      Reason to be suspicious of the wide radius range of 3 to 40. This cluster is smaller though, and outliers could throw off our analysis.
    </div>
</div>
<div style="height: 20px;"></div>

<p>Having considered each cluster, some reflections:</p>
<ul>
  <li><strong>Some relevant patterns, no neat classifications</strong> - Aside from the very-large brown dwarfs, no cluster was a perfect category.</li>
  <li><strong>Clustering could have benefited from more features</strong> - Adding features like orbitals or host star characteristics could have siphoned off clearer groups.</li>
  <li><strong>Clustering is difficult for badly defined categories</strong> - We could have difficulty because of flaws inherent in exoplanet characterization.</li>
</ul>
<div style="height: 20px;"></div>

<p>Expanding on this, it's clear that we tend to designate mass and radius-based categorizations of planets based on our own Solar System rather than the fundamental qualities of planets. It's essentially a form of <strong><a href="https://www.britannica.com/science/heliocentrism" target="blank">heliocentrism</a></strong>. What differentiates a Sub-Neptune and a Super-Neptune aside from their size relative to Neptune? Categorizing planets thousands of light years away in this matter could be argued as unscientific.</p>

<p>The ramifications of bad groupings is that a machine learning classification algorithm may struggle to accurately categorize groups when the categories themselves are not based on the fundamental characteristics of the members. I do believe that this is most likely what's occurred here and for that reason there's a threshold to the success that can be achieved using this approach.</p>

<div style="height: 20px;"></div>

<h3 align="center"> Closing thoughts </h3>
<div style="height: 20px;"></div>

<p>I think that the thrill with data visualization in particular is being able to take complexity and deliver insights that are tangible, and hopefully stimulate curiosity in people they didn't know they had. Obviously it just scratched the surface, but this work challenged me to reflect on representing a scientific domain through data science in a way that felt compelling. </p>
  
<p>As data scientists we tend to get enthralled in the complexity of our work, and sometimes forget the power that lies in the bridges of communication we're able to create with our skills. This project allowed me to tap into this practice.</p>
  
<p>I believe that the machine learning portion could have explored other categorization methods or gone into more depth, but I also think that the categorization issue from the end is limiting. What consists in a good categorization becomes subjective at a certain point.</p>

<div style="height: 20px;"></div>
<hr>
<div style="height: 20px;"></div>
<h4>Resources</h4>
<div style="height: 20px;"></div>

If worked has piqued your interest on exoplanet science, here are a few resources I've enjoyed in creating this project:
<div style="height: 10px;"></div>
<ul>
<li><strong><a href="https://astrobiology.com/2023/05/discovery-of-69-new-exoplanets-using-machine-learning.html" target="blank">Discovery of 69 New Exoplanets Using Machine Learning</a></strong> - Incredible usage of a deep learning algorithm called ExoMiner and the technique of multiplicity to identify new planets.</li>
  <li><strong><a href="https://www.astronomy.com/science/chinas-ambitious-plan-to-find-the-first-earth-2-0/" target="blank">China's Ambitious Plan to Find the First Earth 2.0</a>
</strong> - A look into China's efforts to launch a new telescope in 2026 that will devote itself to finding a habitable Earth-like exoplanet.</li>
<li><strong><a href="https://webbtelescope.org/contents/articles/webbs-impact-on-exoplanet-research" target="blank">Webb's Impact on Exoplanet Research</a></strong> - Dives in to the to-be-seen impact of the James Webb Space Telescope on exoplanet science, which introduced novel exoplanet detection techniques into the fold.</li>
<li><strong><a href="https://www.explore-exoplanets.eu" target="blank">Explore exoplanets: The knowledge server - Exoplanets</a></strong> - A very complete and rich EU-based exoplanet learning hub.</li>
</ul>
<div style="height: 10px;"></div>
Some technical resources and tools I found helpful for this work:
<ul>
  <li>
    <strong><a href="https://www.sonofacorner.com/beautiful-tables/" target="blank"> Beautiful Tables in Matplotlib, a Tutorial</a></strong> and <strong><a href="https://matplotlib.org/matplotblog/posts/how-to-create-custom-tables/?ref=sonofacorner.com" target="blank"> How to create custom tables</a></strong> - Great tutorials on creating custom tables.
  </li>
<li>
<strong><a href="https://matplotlib.org/stable/users/explain/customizing.html" target="blank">Customizing Matplotlib with style sheets and rcParams</a></strong> - Comprehensive from-the-source demo on customizing Matplotlib style.
</li>
  <li><strong><a href="https://projects.susielu.com/viz-palette" target="blank">Viz Palette</a></strong> - Outstanding and flexible palette tool to pick and test out your color schemes.</li>
<li>
<strong><a href="https://python-graph-gallery.com/lollipop-plot/" target="blank">Lollipop chart | The Python Graph Gallery</a></strong> - Short intro to lollipop plot smart barplot alternative to avoid <a href="https://en.wikipedia.org/wiki/Moir%C3%A9_pattern" target="blank">Moiré patterns</a>.
</li>
</ul>





