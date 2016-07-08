#Website Optimization
The objective of this project is to optimize Cameron Pittman's Portfolio website.  There are three parts to this project.  The first task is to optimize the overall website for a PageSpeed score of at least 90 on both Mobile and Desktop by removing the rendering blocks.  The second task is to remove jank from the website-within-a- website, the pizzeria section, which is initiated with pizza.html. The third task is to optimize the code such that the time to resize pizzas is less than 5ms.

##Repository Contents
The main branch of this repo contains a "source code" directory named src, which contains files that include all the changes made to optimize the website's performance.  The html, css, and js files in src are not minified, so that commentary, etc., can be seen.  A "production code" directory, named dist contains the same code as src, but with html, css, and js files minified.

The gh-pages branch contains only the dist code files, so the optimized website may be viewed by browsing http://randyhoffner.github.io/FEND-Proj-4.

##Task 1 -- Optimize Page Loading Speed
The raw code as forked from the Udacity github repo was loaded onto one of my own websites, http:// randyhoffner.net, so that PageSpeed Insights could be run using the public internet.  The original PageSpeed Insight scores were Mobile 28/100 and Desktop 30/100.  Optimizations were implemented, using a combination of PSI suggestions, Udacity lesson videos, recommended web reading, and the Udacity forums: 
  * Resize and compress pizzeria.jpg
  * Losslessly compress profile.pic
  * Inline googleapis font in index.html
  * Defer loading of css/style.css
  * Add media query to css/print.css as it is only needed for printing
  * Async google-analytics.com/analytics.js
  * Async js/perfmatters.js
  * Minifiy all html, css, and js files

  After the above optimizations and file minifications were peformed, PageSpeed scores were raised to 95 on Mobile and 95 on Desktop.
  
  
##Task 2 -- Remove Jank from Cam's Pizzeria Section
The raw code was run in a local server using Python SimpleHTTPServer and NGROK, producing a web address on the local host from which the pizzeria could be run in a browser.  Chrome Canary Dev Tools were then used to observe the presence of long frames.  The following changes were made to views/js/main.js and css/style.css:
  * Line 551 - Number of sliding pizzas was reduced from 200 to 40, as 40 fills all sizes of screen.
  * Line 07 - Replace var items = document.query.SelectorAll('.mover'); with var items = document.getElementsByClassName('mover'); which is a more efficient way to access the DOM.   
  * Line 516 - Using the raw code, layout gets retriggered on each scroll.  It is more efficient to use CSS3 hardware acceleration and transformations, so that only the pixels that actually change as the pizzas slide are retriggered, reducing layout and paint time and avoiding layout thashing.
      * Add transform: translateZ(0);, transform: translateX(); for webkit, moz, ms, and o to the mover class in css/stsyles.css
      * Line 526 - Replace items[i].style.left = items[i].basicLeft + 100 * phase + 'px'; with:
      var left = -iems[i].basicLeft + 1000 * phase + 'px';
        items [i].style.transform = "translateX("+left+") translaeZ(0)";
  * Add backface-visibility: hidden; to mover class of css/style.css. This forces each sliding pizza into its own layer, moving a lot of processing from  the CPU to the GPU.
  * Add will-change: transform; to the mover class of css/style.ss to inform the browser that this parameter will change.
  * Minify views/js/main.js, views/css/styles.css, views/css/bootstrap-grid.css and views/pizza.html.

After the above optimizations were performed using the local server, the code was also tested on the public internet on randyhoffner.net, with similar results to those obtained on the local server.

##Task 3 -- Reduce Pizza Resize Time to Less Than 5ms
The work on views/main.js was done using Chrome Canary Dev Tools with the site running on the local host.  Original time to resize pizza was about 130ms.
  * Add will-change: transform; to randomPizzaContainer:after; class of css/style.css.  Lets the browser know this parameter will change.  This reduced resize time to about 60ms.
  * Line 452 - Within the changePizzaSizes function, move the three variables: i, dx, and newWidth, so that they are outside the for-loop.  This reduced resize time to about 1.5ms.

These changes were checked on the public internet, and resize time was slightly higher, but still  well below 2ms.