## Introduction to PWA| Next.js | PWA Series PART-1


 # What is PWA and how it will help you?

PWA has been around for more than 5 years now, but lately it's popularity is increasing. If you are building a new website and want to get more engagement from your users then PWA is a  must. 

In this two part series I will briefly explain what a PWA is and it's requirements and then in the second article how to create your own PWA. It is quite a simple process and don't need an architecture or design overhaul (in most cases).

# Introduction

> *Progressive web apps are a hybrid of regular [web pages](https://en.wikipedia.org/wiki/Web_page) (or [websites](https://en.wikipedia.org/wiki/Website)) and a [mobile application](https://en.wikipedia.org/wiki/Mobile_app).*

In simple terms PWA or progressive web apps are websites that can act as native apps (The apps we download from app store or play store and have tight integration with hardware). This is amazing because by using certain web API's, PWA can do a vast amount of things which were previously only possible by native apps. Read more about system access capability [here](https://www.simicart.com/blog/pwa-hardware-access/).

# How to identify a PWA capable website?

When you visit a PWA enabled website, you can see the following indications to install it.

### Desktops

![pwa_twitter_eg.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1621764010819/Y7QHiEl8i.jpeg)

### Mobiles

![pwa_milindsoorya-site_eg.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1621764027955/400cYf4Pj.jpeg)

PWA allows web developers to create amazing apps without having to learn kotlin or swift. Also, these companies can save time and effort required to develop for multiple platforms. With proper development PWA can be indistinguishable from native apps, as is evident from some the examples given below.

# Some of the popular PWA’s

1. [Starbucks](https://app.starbucks.com/)
2. [Housing.com](https://housing.com/)
3. [2048 Game](http://2048game.com/)
4. [MakeMyTrip](https://www.makemytrip.com/)
5. [Uber](https://m.uber.com/)
6. [Pinterest](https://www.pinterest.com/)
7. [Spotify](https://open.spotify.com/)

last but not the least, my own website  [MilindSoorya](https://milindsoorya.com/) 😉.

Added benefit :- Using PWA developers don't have to give 30% of their revenue to Google or Apple as commission. 🤑

# A good PWA should satisfy the following criteria's

1. **Starts fast, stays fast** :
Performance plays a significant role in the success of any online experience, because high performing sites engage and retain users better than poorly performing ones. Sites should focus on optimizing for user-centric performance metrics.

2. **Works in any browser** :
Users can use any browser they choose to access your web app before it's installed.

3. **Responsive to any screen size** :
Users can use your PWA on any screen size and all of the content is available at any viewport size.

4. **Provides a custom offline page** : 
When users are offline, keeping them in your PWA provides a more seamless experience than dropping back to the default browser offline page.

5. **Is installable** :
Users who install or add apps to their device tend to engage with those apps more.

The section below is only for those who would like to get a glimpse of how it all works, please note that understanding it's working is not really necessary to implement it. I suggest first implementing it and then reading the resources.

# So how does a PWA work behind the scenes

### Service Workers 👷🏼‍♀️

PWA is made possible because of **Service Workers,** in simple terms a service worker is a script that your browser runs in the background. It's a [JavaScript Worker](https://www.html5rocks.com/en/tutorials/workers/basics/). 

PWA uses the caching and storage APIs available to service workers, to precache parts of a web app so that it loads instantly the next time a user opens it. Using a service worker gives your web app the ability to intercept and handle network requests, including managing multiple caches, minimizing data traffic, and saving offline user-generated data until online again.

![service_worker_eg.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1621764041989/fGiPkSTRQ.jpeg)

### Application Shell (app shell)

As the name implies it contains the local resources that your web app needs to load the skeleton of your user interface so it works offline and populates its content using JavaScript. A service worker then caches the app shell and in repeated visits the app shell make the app load faster. using app shell is not a requirement for PWA but it is recommended.

### Workbox 📦

Workbox is the library that enables PWA creation quite easy. Building a PWA consists of creating a set of service workers and workbox basically automates that task and bakes in a set of best practices and removes the boilerplate every developer writes while creating a service worker.

👉🏼 checkout my website, [milindsoorya.com](https://www.milindsoorya.com) for more updates and getting in touch. cheers.

# Resources

- Introduction to PWA from google: 
[https://developers.google.com/web/ilt/pwa/introduction-to-progressive-web-app-architectures#key_concepts](https://developers.google.com/web/ilt/pwa/introduction-to-progressive-web-app-architectures#key_concepts)
- Introduction to service workers:
[https://developers.google.com/web/fundamentals/primers/service-workers](https://developers.google.com/web/fundamentals/primers/service-workers)
- Web Workers Basics:
[https://www.html5rocks.com/en/tutorials/workers/basics/](https://www.html5rocks.com/en/tutorials/workers/basics/)
- Workbox:
[https://developers.google.com/web/tools/workbox](https://developers.google.com/web/tools/workbox)