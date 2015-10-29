### Getting started

There are two parts to this project.

Part 1: The original project PageSpeed scores the index.html at 67% is for  mobile and 97%. The aim is to increase the score to at least 90% on both decide after fixing.

Part 2: In its original incarnation the file "views/pizza.html" was not optimized for 60fps (views/pizza.html). The aim is to optimize it to 60fps.

####Set-up

1. Check out the repository
2. To inspect the site on your phone, you can run a local server

  ```bash
  $> cd /path/to/your-project-folder
  $> python -m SimpleHTTPServer 8080
  ```

1. Open a browser and visit localhost:8080
2. Download and install [ngrok](https://ngrok.com/) to make your local server accessible remotely.

  ``` bash
  $> cd /path/to/your-project-folder
  $> ngrok 8080
  ```

####Part 1: Optimize PageSpeed Insights score for index.html

Methodology:
   ```bash
   1. Open the PageSpeed site: https://developers.google.com/speed/pagespeed/insights/?url=https%3A%2F%2Fgithub.com%2Fudacity%2Ffrontend-nanodegree-mobile-portfolio%2Findex.html&tab=mobile
   2. Scan this  https://github.com/udacity/frontend-nanodegree-mobile-portfolio/index.html
   3. Follow the instructions relating to improvements
   ```

Improvement list:
   ```bash
   1. Set the view port to the width of the device, to scale it down to the device size.
   Not directly related to performance, but it makes sense to fix
   <meta name="viewport" content="width=device-width">
   2. Remove the following tags and replace by their inline equivalent
   <link href="css/style.css" rel="stylesheet">
   <link href="css/print.css" rel="stylesheet">
   3. addition of the 'async' to as this js dos not contain css modification
   therefore it can be run in parallel.
   <script async src="http://www.google-analytics.com/analytics.js"></script>
   4. I used http://compressjpeg.com/ to compressed all jpg images.
   This reduce the picture foot print by a factor of 4 on average.
   5. I used http://www.willpeavy.com/minifier/ to minify the html
   ```


####Part 2: Optimize Frames per Second in pizza.html

TBD

