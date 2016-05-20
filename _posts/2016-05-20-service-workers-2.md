---
title: "The trouble With Service Workers"
summary: "This post aims to describe the key challenges of developing a service worker. It follows on from yesterday's post about the benefits, and will be followed by more posts on best practices"
layout: default
redirect_from: "/"
---

For most web developers writing service workers is hard.  While you may be experienced at developing for the web, and may even have used a range of more modern Javascript features, very few front-end developers are prepared to just jump into service worker development where modern features are centre-stage and many of the most useful features which have been so useful in the past are gone. In many ways it is more like coding in Node than on the front-end, but with the addition of a bunch of browser-specific issues and capabilities. That doesn't mean it isn't worth it, but it will likely take more trial-and-error and development time than you initially think, and the temptation to give up will be strong. 

Each section in this post outlines a different key challenge you will face in service worker development. 

## There's no HTML or DOM behind you

Most of modern front-end development revolves around manipulating the DOM. In service workers, none of that is even there. In most ways this is a massive relief - millions of browser-specific issues and strange timing problems just melt away. On the other hand, if understanding how to make things appear and disappear, and the frameworks which support this are your bread and butter then you are about to step out into virgin territory. 

In addition, while tools from require.js onwards have pushed away from multiple script includes in HTML, it has remained an effective tool. It is used every day to safely include third-party resources, asynchronously load scripts which don't interfere with each other, and just to prototype. While a subset of these things can still be done, the tools have changed, and your mindset needs to change with it.

## You Can Write In ES6. You Should Write in ES6

Support for service workers is limited to relatively recent versions of Chrome and Firefox. Both of these browsers support ES6 in most of the versions which support ServiceWorkers, and in all their supported versions. This is actually a great opportunity. If you use nothing else from ES6 at the very least you are going to find lambda functions invaluable. The downside is that this opens up a whole new world of potential confusion. 

## Promises

Promises are a key part of the service worker API. Understanding them is essential to making progress in creating one. If you haven't used them before, promises are basically an awesome way to handle asynchronous operation without having to build a tottering pile of callback functions you should go and read the [introduction to them](http://www.html5rocks.com/en/tutorials/es6/promises/) on HTML5 Rocks now. At the same time they are tricky to learn and tricky to use. ES7 has the keywords `async` and `await` which are awesome and will make asynchronous coding with promises *almost* as simple as normal Javascript, but in the meantime you are stuck with a powerful but challenging API. [This post](https://pouchdb.com/2015/05/18/we-have-a-problem-with-promises.html) on MongoDB's blog is a great resource for understanding what you might be doing wrong and putting it right.

## Fetch instead of XMLHttpRequest

Fetching data from the network has changed, and it's one of the things you are going to want to do a lot in service workers. Basically the old XMLHttpRequest API is not available, and has been replaced with the new [Fetch API](https://developer.mozilla.org/en/docs/Web/API/Fetch_API). In general this is a huge improvement. The Fetch API consists of a number of well-designed classes which make use of promises in place of event listeners. The differences can take some learning though.

## Everything is driven by events

service workers can only perform actions in response to events. Unless the browser fires an event such as a notification, or the a page from the site is loaded, the service worker won't even be loaded. Even when a page is loaded it can only make the service worker do anything by requesting a network resource and thereby firing the fetch event, or by sending a message and firing the message event. While this is largely also true of on-page Javascript generally, there is some opportunity there to use the setTimeout function to defer execution, or to leave Javascript running to poll indefinitely on a network resource. In a service worker, the whole Javascript environment will be closed down once an event has been processed, and sooner if the browser determines that the processing is taking too long.

## Use of local storage APIs is essential

This leads us to the final complexity. Although service workers are designed to stay resident on the system, they generally retain no state in memory between events being fired. Anything you need to retain between requests has to be stored in one form of local storage or another. 

Somewhat ironically, the simple localStorage key-value store is not an option here as it is not exposed in the service worker environment. The reason for this is that it is not an asynchronous call and, like Node, all calls to network or file-system resources in service workers must be made asynchronously. You are restricted therefore to [IndexedDb](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API), a NoSQL-style store which allows you to hold key-value pairs with additional indexes. If you are targeting Chrome only you can also use WebSQL. There is also a separate API dedicated to caching network responses. In all these cases the API is much more complex than simple localStorage, with multiple datastores, tables which require defined schemas, and absolutely every call being asynchronous.

## In summary

Service workers are a challenging environment, but it's worth getting to grips with the new technology for two reasons. Firstly you will be able to take advantage of all the cool features service workers enable. And secondly, this is the way the web is going - most everything you learn here will be the normal way to do things in client-side Javascript within a year or two.


