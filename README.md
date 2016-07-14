#Website Optimization
The objective of this project is to optimize Cameron Pittman's Portfolio website.  There are three parts to this project.  The first task is to optimize the overall website for a PageSpeed score of at least 90 on both Mobile and Desktop by removing the rendering blocks.  The second task is to remove jank from the website-within-a- website, the pizzeria section, which is initiated with pizza.html. The third task is to optimize the code such that the time to resize pizzas is less than 5ms.

##Repository Contents
The main branch of this repo contains a "source code" directory named src, which contains files that include all the changes made to optimize the website's performance.  The html, css, and js files in src are not minified, so that commentary, etc., can be seen.  A "production code" directory, named dist contains the same code as src, but with html, css, and js files minified.

To run the project locally using the src code, download the src folder, and run index.html in a browser, and to run the project using the dist code, do the same using the dist folder.  To directly run the pizzeria section, run views/pizza.html in a browser.  To run in a local server, open a terminal window or a windows PowerShell window, navigate to the appropriate directory, and run Python SimpleHTTPServer.  Open a second terminal window, navigate to the appropriate directory, and use ngrok to provide a local website that can be run in a browser.

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

  After the above optimizations and file minifications were peformed, PageSpeed scores were raised to above 90 on both Mobile and Desktop, when run in a local server or on my website.
  
  
##Task 2 -- Remove Jank from Cam's Pizzeria Section
The raw code was run in a local server using Python SimpleHTTPServer and NGROK, producing a web address on the local host from which the pizzeria could be run in a browser.  Chrome Canary Dev Tools were then used to observe the presence of long frames.  The following changes were made to views/js/main.js and views/css/style.css:
    * Line 477 - Simplified the for-loop, removing determineDx, which was a source of layout retriggering.
    * Add transforms to css/styles.css.
     * Add backface-visibility: hidden; to mover class of views/css/style.css. This forces each sliding pizza into its own layer, moving a lot of processing from  the CPU to the GPU.
  * Add will-change: transform; to the mover class of css/style.ss to inform the browser that this parameter will change.
  * Line 576 - The following changes were made to this function:
		* Columns and rows are calculated dynamically depending on the width and height of the viewport, generating the appropriate number of pizzas for any given viewport size.  Variable "totalPizzas" replaces "200"
		* Change document.querySelector("#movingPizzas1") to the more efficient document.getElementsById("movingPizzas1"), and move this DOM call outside the for-loop.
		* Set for-loop to iterate over "totalPizzas".
  * Minify views/js/main.js, views/css/styles.css, views/css/bootstrap-grid.css and views/pizza.html.

After the above optimizations were performed using the local server, few or no long frames were typically obsefved.  The code was also tested on the public internet on randyhoffner.net, with similar results to those obtained on the local server.

##Task 3 -- Reduce Pizza Resize Time to Less Than 5ms
The work on views/main.js was done using Chrome Canary Dev Tools with the site running on the local host.  Original time to resize pizza was about 130ms.
  * Add will-change: transform; to randomPizzaContainer:after; class of views/css/style.css.  This lets the browser know this parameter will change.  This reduced resize time to about 60ms.
  * Line 456 - function changePizzaSizes(size) was changed to work with percentages.  
  * Line 475 - Change DOM call document.querySelectAll() to the more efficient document.getElementsByClassName("randomPizzaContainer") and cache the DOM call as a variable outside the for-loop, avoiding multiple DOM calls.
Resize pizzas typically takes less than 1ms.  These changes were checked on the public internet with similar results.