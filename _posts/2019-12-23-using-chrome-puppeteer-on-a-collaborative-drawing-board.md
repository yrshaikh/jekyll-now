
## Using Chrome Puppeteer on a collaborative drawing board

![](https://cdn-images-1.medium.com/max/2000/1*EpLnGAR4-1BP1LyK-iU2JA.png)

To add some more fun at work, we have made it a tradition at [Agoda](https://www.agoda.com) to host a collaborative drawing board in the last week of the year. To my fellow readers who do not know what a collaborative drawing board means…
>  It is a medium that provides the ability to draw sketches together with your friends and colleagues in real-time, over the internet, in your browser.

So this time when I saw this message in our team chat, I quickly tried to find the piece of code I’d written the last time using Chrome Puppeteer.

![](https://cdn-images-1.medium.com/max/2000/1*v04z7ApDWe6PowoRP6l8LA.png)

![Could not find it!](https://cdn-images-1.medium.com/max/2388/1*WhMwQ3e2I0-Uuv2s0KcsXQ.png)

I was annoyed by not finding it (again!), so this time I decided come what may, I will now

 1. Write a blog post about it

 2. Post the code to GitHub, so that I never lose it. (Though the piece of code is super trivial and takes only a few minutes to write, not being able to find when you need it is even more painful.)

![Photo by [Ryan Franco](https://unsplash.com/@ryanmfranco?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)](https://cdn-images-1.medium.com/max/7460/0*aSxUwt2cr-IDF_m9)

So here we go!

### **Getting started**

 1. I use VS Code which has integrated terminal support, using which I run yarn add puppeteer to install puppeteer. (At the time of writing this post the node version on my machine is 10.13.0)

 2. Once installed, you will see a package.json file will be created with puppeteer as a dependency.

 3. Next is to create an index.js file (which for now will have a very basic code using puppeteer) to load a page and then take a screenshot.

    const puppeteer = require("puppeteer");

    async function run() {
        const browser = await puppeteer.launch();
        const page = await browser.newPage();
        await page.goto("https://google.com");
        await page.screenshot({ path: "google.png" });
        browser.close();
    }

    run();

4. Then we run this file by using node index.js

5. Once the run finishes, we can see VS Code has already identified a new file in the explorer google.png

![Screenshot of the page taken using Chrome Puppeteer.](https://cdn-images-1.medium.com/max/3286/1*E6ZNZCP5nulaDir1CfHLwA.png)

So now we have managed to get a H*ello World *kind of program up and running using chrome puppeteer.

### Visual debugging

Before we continue, I am going to add a setting to the launch method of puppeteer called headless:false to enable visual debugging.

    const browser = await puppeteer.launch({ headless: false });

### Let’s begin by drawing a simple box

![](https://cdn-images-1.medium.com/max/2000/1*jT0meljZSsjrw6M5f5zZ8g.png)

To draw a box, we need to select 4 (x,y) coordinates. For this example, here are the 4 coordinates I have selected — (50, 50) → (500, 50) → (500, 500) → (50, 500).

I will be using the [*mouse class](https://github.com/puppeteer/puppeteer/blob/v2.0.0/docs/api.md#class-mouse)* to move and click at different coordinates. The *mouse *class operates in main-frame CSS pixels, relative to the top-left corner of the viewport.

I have also added a 500 ms delay between each click to visually see the box being drawn.

    *const* puppeteer = require("puppeteer");
    *const* delay = *time* *=>* new Promise(*res* *=>* setTimeout(res, 500));

    async *function* run() {
       *const* browser = await puppeteer.launch({ headless: false });
       *const* page = await browser.newPage();
       await page.goto("xxx");

       await mouseClick(page, 50, 50);
       await mouseClick(page, 500, 50);
       await mouseClick(page, 500, 500);
       await mouseClick(page, 50, 500);
       await mouseClick(page, 50, 50);
    }

    async *function* mouseClick(*page*, *x*, *y*) {
       await delay();
       await page.mouse.click(x, y);
    }

    run();

Below is the output on running the above piece of code —

![Drawing a basic box using chrome puppeteer](https://cdn-images-1.medium.com/max/2000/1*a5Ub9w-HgsHdrw0e789zXQ.gif)

Now that we have got this working, let’s go ahead and add a little bit of complexity. We’ll modify the logic and keep drawing smaller inner boxes until we run out of space.

    *let* x = 50;
    *let* y = 500;

    while (y > 0) {
       await mouseClick(page, x, x);
       await mouseClick(page, y, x);
       await mouseClick(page, y, y);
       await mouseClick(page, x, y);
       await mouseClick(page, x, x);

       x = x + 2;
       y = y - 2;
    }

![Drawing infinite boxes](https://cdn-images-1.medium.com/max/2000/1*37qUcg-_Os1rkzrjRhPuFQ.gif)

And that’s just the beginning of the things one could do using Chrome puppeteer. As promised, posting a link to my code (and hope to add more examples) on Github down below —
[**yrshaikh/puppeteer-draw**
*You can’t perform that action at this time. You signed in with another tab or window. You signed out in another tab or…*github.com](https://github.com/yrshaikh/puppeteer-draw)
