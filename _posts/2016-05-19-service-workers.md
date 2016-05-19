---
title: "Why you want a Service Worker"
layout: default
---

# Why you want a Service Worker

###### This post outlines the benefits of having a service worker on your site. There will be follow-up posts on the challenges of developing a service worker, and on what I think is good practice in solving problems you will encounter.

If you read the articles from Google, you will learn that service workers are the latest way to turn your website into an applciation, and you'll probably think &lsquo;so far so 1998&rsquo;. So what is it about service workers that makes them so different? 

Well, the short answer is the environment. On the desktop, at least since broadband became the norm, being a web-app doesn't actually mean very much. Most web use is now taking place on mobile though, and even the static experience is starting to be mostly on tablets. In these environments people expect different things; the app culture has promoted a fuller experience, with location-awareness, notifications and home-screen integration, while at the same time people expect to still be able to work even when their internet connection is slow and patchy.

Service workers are the answer to these problems. Essentially, while they avoid the overhead of downloading and installing an app, they offering a permanent beachhead on the device to a website or service, allowing them to work offline and respond to events even when the user isn't currently on the website.

In terms of support, Google is the key supporter of the standard, with a full implementation in Chrome on Android all but the oldest Android devices to support them out of the box. Mozilla also have a full implementation on desktop, with many features supported on Android and more timetabled for development right now. Even [Microsoft are working on it](https://developer.microsoft.com/en-us/microsoft-edge/platform/status/serviceworker), so it should make an appearance sooner or later on Edge. Right now the only hold-out is Safari where there has been some consideration, although it plays quite strongly against the traditional Apple walled-garden approach.

In short Service Workers let you focus on the web interface, but still provide an app-like experience for Android without necessarily going to the overhead of developing an app. On Safari your users will still get the experience of a well-developed website, and on desktop you can push new capabilities like notifications which users may learn to love.

## What are the problems a service worker can solve

It's still early days, but there are four key problems a service worker can solve for you right now:

- *Desktop integration.* In combination with a web manifest file, you can provide users with the option to store an icon to launch your site on their homescreen. When the site is launched through this icon it can display a customized splash screen, launch full-screen without a URL bar or other browser chrome, and it appears as a separate process in the task bar. Sites meeting the full criteria will even get a prompt in Chrome to suggest users install them.

- *Running offline.* This is the number one capability you gain from a service worker. You can choose to cache any and all of your assets in such a way that they are available when there is no internet connection. This means that when a user hits your URL, or even better clicks on the desktop icon discussed above, they will hit your site and be able to view already-downloaded content or use javascript features just as though they were online.

- *Minimizing HTTP round-trips.* Even a user who has visited your site before is likely to need to hit the server for every asset involved in displaying your page. Browser-level caching will mean that all they are getting back in most cases is a series of content-less 304 responses, and HTTP/2 means that the requests are smaller and can be pipelined efficiently, but there is still going to be latency. When the connection is poor or intermittent this could add 10s of seconds to your page load time. 

- *Notifications.* For most users the number one benefit of applications over websites is notifications. These let people get on with their lives without constantly having to check services for new messages or content. At the same time they prevent applications from having to poll for updates, saving battery life and again providing a more effective service on patchy connections. In the web-app case notifications let you provide these benefits to users even when they aren't on your site. At the same time they can save you from the expense of holding open websockets for intermittent updates.

## What might be possible in the future

As browser providers get more comfortable with the technology, and figure out ways to ensure that user permissions are respected, they are likely to open up various other triggers for service workers to perform actions on a user's phone. These may include:

- *Geolocation changes.* The ability to spot when a user has travelled from one locality to another means that you can trigger updates and notifications relating to available buses in the new area for example. Allowing this permission to selected websites is something that browser-makers are rightly wary of, but if implemented well it would eliminate one more major reason to have a native app.

- *Periodic alarms.* The obvious function here is calendar apps, but this could also provide a new dimension to thick client Javascript apps, and reduce the need for a server to collate data from different providers.
