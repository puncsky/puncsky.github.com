---
layout: post
title: "Lesson Learned from Development of Just2do"
date: 2013-09-22 18:26
comments: true
categories: web_development
published: false
---

Throughout this summer, I has been building a web application for the social todo list. Many developers, just as I used to be, might say that building a website is the *simplest* job in the programming world, and that the reason most people want to program for the web is that they're *not smart enough* to do anything else. Unfortunately, this idea is [too simple and too naive](http://www.youtube.com/watch?v=gSz-bzXCbMI).

<p align="middle"><img src="https://raw.github.com/puncsky/puncsky.github.com/source/images/just2do.png" alt="just2do.it"></p>

Why is it stupid? Web is the future. In addition to the general advantages, web app is way more open, both for more users and for more machines. Native software applications simply do not have url, which means it is rather hard to share. As for me, it is unreasonable that I cannot search the application itself. It takes me minutes to find tabs for email / calendar /... in a desktop Microsoft Outlook. However, if it is a web app, I can make it in seconds with `ctrl + f`. Every word on web is selectable and searchable. Most importantly, desktop app means nothing in the new era of data mining, whereas webpages come alive for the reason of the web crawler and the search engine, especially when it is the semantic web.

If you have to choose between Chrome and MS Office, which one do you prefer? My choice is chrome. Since we can render almost everything with Chrome, why do we bother to buy a fat-and-slow MS Office?

As for web development, I used to build a online shopping website in three days and, of course, would think that was easy. However, through the work on Just2do, I resolve to give up the stupid idea. In the last two month, I got stuck in enormous situations... I should have completed this task over one month ago. Since I got stuck, maybe it is time to rethink about it. In summary, I learn that

1. **Never underestimate the cost of developing a user-friendly action-intense website.** When building native apps, one or two programming languages are enough. However, to craft a truly beautiful and useful website, that number is too idealistic.

2. **Record time in UTF.** The users are coming from around the globe. It is better to consider timezones early, with every time data saved in UTF.

3. **Use MVVM instead of controller-fat MVC.** Web server is not the only thing to be considered. Sometimes, [it is still inevitable for the server to send data to native apps](http://techcrunch.com/2012/09/11/mark-zuckerberg-our-biggest-mistake-with-mobile-was-betting-too-much-on-html5/). So leaving space for REST-like services can make our lives easier. **Model layer can serve both the web UI and the native app UI.**(see the end of this post). To have a better understanding of MVC, I highly recommend this link  (in simplified Chinese)[MVC之患上肥胖症的Controller](http://www.lovelucy.info/fat-controller-bad-mvc.html).
4. **Ponder over actions and the corresponding partial views.** Just2do is an actions-intensive web app. Many actions will update target partial views asynchronously. It is better to list all the possible actions first and then to coin the site.

5. [Any application that can be written in JavaScript, will eventually be written in JavaScript.](http://www.codinghorror.com/blog/2009/08/all-programming-is-web-programming.html) Learn javascript for the sake of future.

Finally, I would like to mention the amazing tools:

- [Bootstrap](http://getbootstrap.com/): Responsive Front-End Framework.
- [Chosen](http://harvesthq.github.io/chosen/): a jQuery plugin that makes long, unwieldy select boxes much more user-friendly.


<p align="middle"><img src="https://raw.github.com/puncsky/puncsky.github.com/source/images/webdev.png" alt="Web Development"></p>

