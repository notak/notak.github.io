## Why this is nothing like the rest of the web

While you may be experienced at developing for the web, and may even have used a range of more modern Javascript features, very few front-end deveopers are prepared to just jump into service worker development where modern features are centre-stage and many of the most useful features which have been so useful in the past are gone. In many ways it is more like coding in Node than on the front-end, but with the addition of a bunch of browser-specific issues and capabilities.

### There's no HTML or DOM behind you

Most of modern front-end development revolves around manipulating the DOM. In service workers, none of that is even there. In most ways this is a massive relief - millions of browser-specific issues and strange timing problems just melt away. On the other hand, if understanding how to make things appear and disappear, and the frameworks which support this are your bread and butter then you are about to step out into virgin territory. 

In addition, while tools from require.js onwards have pushed away from multiple script includes in HTML, it has remained an effective tool. It is used every day to safely include third-party resources, asynchronously load scripts which don't interfere with each other, and just to prototype. While a subset of these things can still be done, the tools have changed, and your mindset needs to change with it.

### The API is different.
- Fetch
- Promises

### Local storage is essential

### Everthing is driven by events

