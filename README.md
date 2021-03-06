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
   1. Image Size Optimisation
      In pizza.html the image is displayed as 360 x 270.
      The actual image is 4+ times that size.
      I reduce the size of the image and compress it online: http://www.imageoptimizer.net/Pages/Home.aspx
      Original Image  2048 x 1536  - 2314  KB
      Optimized  360 x 270  - 105  KB
      Pizza.jpg width amended from 360px to 100px
   2. Google Font Optimsation.
      http://fonts.googleapis.com/css?family=Open+Sans:400,700 points at CSS on Google.
      I have copied that CSS and place it between style tags in the head of the index.html document to inline it.
   3. Open the PageSpeed site: https://developers.google.com/speed/pagespeed/insights/?url=https%3A%2F%2Fgithub.com%2Fudacity%2Ffrontend-nanodegree-mobile-portfolio%2Findex.html&tab=mobile
   4. Scan this  https://developers.google.com/speed/pagespeed/insights/?url=https%3A%2F%2Fgithub.com%2FFredGH%2FPerformanceImprovement%2Findex.html
   5. Both Mobile and Desktop Speed are > 90%
   ```

Improvement list:
   ```bash
   1. Set the view port to the width of the device, to scale it down to the device size.
   Not directly related to performance, but it makes sense to fix
   <meta name="viewport" content="width=device-width">
   2. Removed the following tags and replace by their inline equivalent
   <link href="css/style.css" rel="stylesheet">
   <link href="css/print.css" rel="stylesheet">
   3. Added  the 'async' to as this js dos not contain css modification
   therefore it can be run in parallel.
   <script async src="http://www.google-analytics.com/analytics.js"></script>
   4. Used http://compressjpeg.com/ to compressed all jpg images.
   This reduce the picture foot print by a factor of 4 on average.
   5. Used http://www.willpeavy.com/minifier/ to minify the html
   ```

####Part 2: Optimize Frames per Second in pizza.html

   ```bash
   1. According to the https://jsperf.com/getelementsbyclassname-vs-queryselectorall/18, the getElementsByClassName is way faster than querySelectorAll.
   Consequently all instances of querySelectorAll have been replaced by  getElementsByClassName.
   ```

   ```bash
   2. The below loop is slow due to generation of the phase param each time within the loop

    var items = document.getElementsByClassName('.mover');
     for (var i = 0; i < items.length; i++) {
       var phase = Math.sin((document.body.scrollTop / 1250) + phaseArr[i % 5]); <========= slow
       console.log("Phase: " + phase );
       items[i].style.left = items[i].basicLeft + 100 * phase + 'px';
     }

   using  'console.log("Phase: " + phase );' in this original loop shows that in  the variable phase repeats the same values many times.
   It is cheaper to load these values into an array in an extra external loop and get read from the array
   It is faster and lower the memory foot print
   Phase: 0.4226466415713652
   Phase: 0.9909775248482073
   Phase: 0.6482082419066111
   Phase: -0.29052070927845763
   Phase: -0.9621462601578072
   ```
   ```bash
   4. As the .randomPizzaContainer size and size are constants, no need to loop
        var dx = determineDx(document.getElementsByClassName(".randomPizzaContainer")[0], size);

     Original code
         for (var i = 0; i < document.getElementsByClassName(".randomPizzaContainer").length; i++) {
              var dx = determineDx(document.getElementsByClassName(".randomPizzaContainer")[i], size);
              var newwidth = (document.getElementsByClassName(".randomPizzaContainer")[i].offsetWidth + dx) + 'px';
              document.getElementsByClassName(".randomPizzaContainer")[i].style.width = newwidth;
            }

     Improved version
        var dx = determineDx(document.getElementsByClassName("randomPizzaContainer")[0], size);
          for (var i = 0; i < document.getElementsByClassName(".randomPizzaContainer").length; i++) {
               var newwidth = (document.getElementsByClassName(".randomPizzaContainer")[i].offsetWidth + dx) + 'px';
               document.getElementsByClassName(".randomPizzaContainer")[i].style.width = newwidth;
             }
   ```
   ```bash
   5. Used the translateZ and translate3d on the '.move' class in style.css to reduce painting.
   ```

   ```bash
   6. Use of Autoprefixer in style.css
      Addition of the backface-visibility property with the hidden in style.css, .mover class
      Restored the z-index: -1 in style.css, .mover class

   7.  'use strict'; tag at the top of the main.js

   8. Removed all leading dot is in DOM calls, in the main.js

   9. Saved array length as a variable outside of each loop, in the main.js

   10. Created a local variable to save  document.getElementsByClassName('randomPizzaContainer'), in the main.js

   11. Declared the pizzasDiv variable outside of the loop, in the main.js

   12. The number of background pizzas  reduced to the size of the screen

   13. Replace  document.querySelector("#movingPizzas1").appendChild(elem); by
       var elem = document.createElement('img');
       ...
       movingPizzas.appendChild(elem);
   ```