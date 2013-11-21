---
title: The Single Page Mindset
layout: chapter
index: 1
tags: ['chapter']
---

When I started dabbling with programming (around the year 2000), [Bruce Eckel](http://www.mindviewinc.com/Index.php) was one of my favorite technical authors. [Thinking in C++](http://www.mindview.net/Books/TICPP/ThinkingInCPP2e.html) was one of the first great programming books that I read back in the days. Apart from the fact that you could read his books online for free, Bruce always taught you to _think_ in a language. His books were in stark contrast to other reference books like [C++ The Complete Reference](http://www.amazon.com/The-Complete-Reference-5th-Edition/dp/0071634800) that covered almost everything about the language but did not show you how to think in a particular language. 

I had a similar experience when I started working with Backbone.js a couple of years back. Having been a Ruby on Rails programmer for many years, I could easily understand what Backbone.js offered - Models, Views and Collections. However, I struggled with the Single Page _mindset_. I had many questions - How do I structure my code? How do I test drive my code? What patterns should I follow for building a desktop like app? This book should help you think about and answer some of these questions and get you into the _Single Page Mindset_.

However, before we talk more about the Single Page Mindset, let's quickly talk about traditional JS enhanced apps.

## Traditional Javascript enhanced Apps

Traditionally, Javascript has been used to enhance the UX (user experience) of an app. A typical web-app (for example the older Basecamp or Highrise) renders pages on the server and sends them back to the browser. The browser simply display it to the user. Javascript is used as and when needed - form validations, click event handlers etc. Since the rise of Web 2.0, developers started using Javascript for AJAX to fetch data (JSON/XML) or HTML and updating sections of the page in the browser without a full page reload. Let's look at a simple code example where we fetch a HTML segment and update the page (this pattern was very popular with Rails app a few years back)

<<[Updating a DIV using jQuery](code/simple_ajax_update.js)

You can interact with an API in a similar way. Only in the case of an API, the succcess method needs to parse the response and then do whatever is needed. If there are multiple sections of the page that need an update, you have to keep track of them and update them. If you have a page listing several products and you want to fetch and update the price for every product, the success handler will now look something like

<<[Updating Multiple DIVs using jQuery](code/multiple_ajax_updates.js)

## Single Page Apps

While JS enhanced Apps do most of the page rendering on the server side and send back the HTML, single page apps do most of their rendering on the client side (that means in the browser). Once you load a single page application in the browser, there is no full page reload and all the updates on the page happen without a page reload. [Trello](https://trello.com/), the collaborative project management software is a good example of this paradigm.

## Multi-screen single page apps

Finally, there is a class of single page apps that involves fairly significant screen updates as you work with them. Different screens have distinct (bookmark-able) URLs just like a traditional JS enhanced apps. A good example is [Gmail](https://gmail.com). Your inbox, a single email, settings and contacts are different bookmarkable screens. You can go back and forth between these screens in Gmail without reloading the page in the browser. Our own application, [SupportBee](https://supportbee.com) is a multi-screen single page app too.

## Thinking the Single Page Way

Let's look at the second example where we update multiple product divs based on the JSON response that we get. There are several problems with code

* The DIVS are identified by the ``product_id``. To be able to do so, you have to render the divs with ``name=product_id``.
* If some code on the page updates the price of a product (let's say by applying a discount code), it has to remember to update the price of that product. If multiple products are updated, you have to iterate over the collection and update the price for each one of them. The amount of book keeping that you do keeps growing as your app grows.

As we go over the rest of the book you will be exposed to the Backbone.js way of handling these issues. To keep things sane, Backbone uses a MVC (Model, View, Collection) pattern and keeps data and logic and presentation separated. Also, extensive use of event binding ensures that you don't have to manually remember to update HTML whenever you change the data. Without getting ahead of ourselves and without diving into any Backbone.js specific code, let's see how we would conceptually handle this situation in Backbone

* We will create a product model which will contain attributes like price, title etc
* We will create a Backbone collection (since there are many products)
* We will create a product view class and create an instance of it for every product that we have (in the collection).
* Using event binding, we will make sure that whenever a product's price changes (independent of where/how it's changed), the new price is reflected in the view.

## Unique challenges faced by single page apps

Single Page Apps are certainly harder to build (atleast when you are just starting out with a new project). There are several unique challenges faced by them. The biggest one is that you are now giving up on many features that browsers provide to typical web applications. The biggest one being a clean state that is offered on every page reload. For example, if you were building Gmail and a user could drop a label on an email when viewing the email, you would have to make sure that on going back to the inbox, the label immediately shows up. In a conventional web-app, going back to the inbox would reload the page and fetch the new inbox page from the server. This would guarantee that the new email listing has the ticket with the right label. 

Another browser feature that users heavily depend on is the back button. Many single page applications break this expectation. Special care must be taken (that is code must be written!) to make sure that the back button works as expected in these applications. Finally, many users are used to copying the URLs for specific screens or bookmarking them for easy access later. If special care is not taken, screens in a single page application might not have a unique URL.
