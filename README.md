Installation
Download. Unzip. And run.

Part 1 - Solution - PageSpeed
Minify CSS and HTML placed into page head. Except for print css which now uses media tag. To minimise amount of downloads
Change JS loading to async
Change image format, size and compression

Part 2 - Solution - Pizza FPS
Change Pizza Size

Originally I just removed some of the code to make it resize faster. My next idea was to create 3 classes and apply the new classes + remove old classes to resize pizza which bought it down to about 4-8ms to resize pizza.

Finally I realised I don't have to have JS cycle through the DOM changing classes. Instead of can just create and modify the class of the container and use CSS specification to change the sizes which is much faster.

Background parallax scroller

Background creation

Orignally 200 pizzas were being created no matter how many were to be displayed on screen. So I changed this to instead check the screen size, and ensure that only enough pizza divs were created to fill the screen. On window resize it deletes the pizzas, recalulates how many is needed and creatres them again.

One of the first optomisitions I made for movement was to use 'transform: translateX' instead of 'styel.left'. There were a number of website that claims this would make it much faster and so I went with it. However in later testing I found it to be generally the same speed or slower, especially in other browsers.

Maybe the most interesting thing I saw from testing was that each browser and platform can find different JS code faster. For example, using translateX was over 50% slower on IE11. On chrome mobile, the test that had been coming last on desktop browsers was the fastest. On chrome desktop none of the rewrite made any real difference.

JSPref test can be found at: http://jsperf.com/udacity-optimise-loop-test/2 Screenshots of FPS using devtools:

Using style.left: http://ro-savage.github.io/udacity-optimise/views/images/style-left.jpg
Using translateX: http://ro-savage.github.io/udacity-optimise/views/images/translate.jpg
Another realisation was that when optomising code, you need to check each optimisation individual if you haven't tried them before. I just assumed translateX would be faster because that is what it was telling me and then made other optimisations at the same time. Thus I couldn't see what change was making improvements when I was testing the code.

I also tested the best way to grab a DOM object and found getElementByID and getElementsByClass were must faster than the new querySelector method: http://jsperf.com/getelementbyid-vs-queryselector/137 I therefore updated all the selectors to use the old school method.

By far the most important optimisation for FPS in Chrome was a CSS hack by David Walsh (http://davidwalsh.name/translate3d). It forced each pizza to be in it's own composite layer and this saw a huge speed increase. When I first tried this it didn't work as I applied the translateX inline and therefore was appearing after page load. It turned out before Chrome rendered the DOM CSSOM it needed to know that this were 'transform' layers.

While the final code may not be as future proof, as the assignment was to optimise for speed I made the decision to go with style.left over translate. Not using translate also means losing the subpixel movement, however in practise as this is a background image moving in a horizontal line it doesn't make much difference.

Finally for download speed, the CSS and JS was minified and placed onto the single page. Images were compressed and resized.

See code for more comments.



Website Performance Optimization portfolio project

Your challenge, if you wish to accept it (and we sure hope you will), is to optimize this online portfolio for speed! In particular, optimize the critical rendering path and make this page render as quickly as possible by applying the techniques you've picked up in the Critical Rendering Path course.

To get started, check out the repository, inspect the code,

Getting started

Part 1: Optimize PageSpeed Insights score for index.html

Some useful tips to help you get started:

Check out the repository
To inspect the site on your phone, you can run a local server

$> cd /path/to/your-project-folder
$> python -m SimpleHTTPServer 8080
Open a browser and visit localhost:8080

Download and install ngrok to make your local server accessible remotely.

$> cd /path/to/your-project-folder
$> ngrok 8080
Copy the public URL ngrok gives you and try running it through PageSpeed Insights! More on integrating ngrok, Grunt and PageSpeed.

Profile, optimize, measure... and then lather, rinse, and repeat. Good luck!

Part 2: Optimize Frames per Second in pizza.html

To optimize views/pizza.html, you will need to modify views/js/main.js until your frames per second rate is 60 fps or higher. You will find instructive comments in main.js.

You might find the FPS Counter/HUD Display useful in Chrome developer tools described here: Chrome Dev Tools tips-and-tricks.

Optimization Tips and Tricks

Optimizing Performance
Analyzing the Critical Rendering Path
Optimizing the Critical Rendering Path
Avoiding Rendering Blocking CSS
Optimizing JavaScript
Measuring with Navigation Timing. We didn't cover the Navigation Timing API in the first two lessons but it's an incredibly useful tool for automated page profiling. I highly recommend reading.
The fewer the downloads, the better
Reduce the size of text
Optimize images
HTTP caching
Customization with Bootstrap



