## How to add Next.js 11 image component to your website

 Next.js gives you the best developer experience with all the features you need for production: hybrid static & server rendering, TypeScript support, smart bundling, route pre-fetching, and more. No config needed.

With the latest version of Next.js, this react framework has become even more desirable due to addition of features to accelerate performance and developer collaboration.

In this article, we'll discuss:

1. New Innovations in Next.js 11
2. **next/image**  Improvements
3. Adding Next.js 11 image component to your project 
4. Using next/image with MDX

So let's dive in.


## New Innovations in Next.js 11 

- [**Conformance**](https://nextjs.org/blog/next-11#conformance): A system that provides carefully crafted solutions to support optimal UX.
- [**Improved Performance**](https://nextjs.org/blog/next-11#improved-performance): Further optimisations to improve cold startup time so you can start coding faster.
- [**`next/script`**](https://nextjs.org/blog/next-11#script-optimization): Automatically prioritise loading of third-party scripts to improve performance.
- [**`next/image`**](https://nextjs.org/blog/next-11#image-improvements): Reduce layout shift and create a smoother visual experience with automatic size detection and support for blur-up placeholders.
- [**Webpack 5**](https://nextjs.org/blog/next-11#webpack-5): Now enabled by default for all Next.js applications, bringing [these benefits](https://nextjs.org/blog/next-10-2#webpack-5) to all Next.js developers.
- [**Create React App Migration (Experimental)**](https://nextjs.org/blog/next-11#cra-migration): Automatically convert Create React App to be Next.js compatible.
- [**Next.js Live (Preview Release)**](https://nextjs.org/blog/next-11#nextjs-live-preview-release): Code in the browser, with your team, in real time.

## **next/image** Improvements

The improvements made to next/image component is  my favourite addition. In this latest iteration of Next.js, image component has been updated to reduce  [Cumulative Layout Shift](https://vercel.com/blog/core-web-vitals#cumulative-layout-shift) and create a smoother visual experience.

I use MDX for my [blog](https://www.milindsoorya.site) and serves the images from the public folder, one downfall of this approach was that to overcome Cumulative Layout Shift and to use Next.js image optimisations I had to give image width and height in the next/Image component. I did this by running a script that will traverse through my blog and convert the markdown images to Next.js images and find the image dimensions. 

**Adding image to markdown**
```markdown
![image info](./static/image.png)
```
**After running the script converted into**
```jsx
<Image
  alt={`image info`}
  src={`/static/image.png`}
  width={5000}
  height={3313}
/>
```

This was a lot of work for displaying an image. With Next.js 11 this has become much simpler, no more scripts!! Yeah..

## Adding Next.js 11 image component to your project 

**STEP 1**  - First step is to update your project to Next.js 11.

   **Upgrade React version to latest**
   With Next.js 11 the minimum React version has been updated to 17.0.2.
   To upgrade you can run the following command:
   ```shell
   npm install react@latest react-dom@latest
   ```
   Or using `yarn`:
   ```shell
   yarn add react@latest react-dom@latest
   ```
   
   **Upgrade Next.js version to latest**
   To upgrade you can run the following command in the terminal:
   ```shell
   npm install next@latest
   ```
   Or using `yarn`:
   ```shell
   yarn add next@latest
   ```

**STEP 2**  - Now you can just import your **local** image and not bother about providing dimensions.
   ```jsx
   import Image from 'next/image'
   import author from '../public/me.png'
   
   export default function Home() {
     return (
       // When importing the image as the source, you
       // don't need to define `width` and `height`.
       <Image src={author} alt="Picture of the author" />
     )
   }
   ```
   Isn't that just amazing!!

**STEP 3**  - Adding Image placeholders
  
![placeholder.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1624721863811/zg-9DYp-e.gif)

This is the feature that really interested me, before this feature if we had to implement this feature, we had to save a low quality image and serve the image and wait for the full size image to load and then load the full size image etc.... in-short it was complicated 😵. 
Now it is as simple as adding a placeholder prop to the component.
```jsx
   import Image from 'next/image'
   import author from '../public/me.png'
   
   export default function Home() {
     return (
       // When importing the image as the source, you
       // don't need to define `width` and `height`.
       <Image src={author} alt="Picture of the author" placeholder="blur"/>
     )
   }
```
If you are using a CDN to serve images, Next.js also supports blurring dynamic images by allowing you to provide a custom `blurDataURL`, which is provided by your back-end. For example, you can generate a [blurha.sh](http://blurha.sh/) on the server. 
```jsx
<Image
  src="https://nextjs.org/static/images/learn.png"
  blurDataURL="data:image/jpeg;base64,/9j/2wBDAAYEBQYFBAYGBQYHBwYIChAKCgkJChQODwwQFxQYGBcUFhYaHSUfGhsjHBYWICwgIyYnKSopGR8tMC0oMCUoKSj/2wBDAQcHBwoIChMKChMoGhYaKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCj/wAARCAAIAAoDASIAAhEBAxEB/8QAFQABAQAAAAAAAAAAAAAAAAAAAAb/xAAhEAACAQMDBQAAAAAAAAAAAAABAgMABAUGIWEREiMxUf/EABUBAQEAAAAAAAAAAAAAAAAAAAMF/8QAGhEAAgIDAAAAAAAAAAAAAAAAAAECEgMRkf/aAAwDAQACEQMRAD8AltJagyeH0AthI5xdrLcNM91BF5pX2HaH9bcfaSXWGaRmknyJckliyjqTzSlT54b6bk+h0R//2Q=="
  alt="Picture of the author"
  placeholder="blur"
/>
```
   **Learn more** [Next.js Image improvements](https://nextjs.org/blog/next-11#image-improvements)

## Using next/image with MDX
This is how I implemented `next/image` component, It is extremely simple. 
**STEP 1** - Create a custom image component in your `MDXComponents` file
This is because I couldn't figure out how to import image to a `MDX` file, so I worked around it by creating a react component  and importing the image in there and and calling that component in the `MDX` file like the code below.
```jsx
// MDXComponets.js
import Image from 'next/image';

const NextImg = (props) => {
  const { src, alt } = props;
  return <Image src={require(`../public/static/images/${src}`)} alt={alt} placeholder="blur" />;
};

const MDXComponents = {
  NextImg,
};

export default MDXComponents;
```
> NOTE - `placeholder={"blur"}` wont work with `gif` files, so I used a `disablePlaceholder` prop to control whether to show the placeholder

**STEP 2**  - If you don't have a `MDXComponents` file, create one in your components folder and call it in your `MDXProvider` in `_app.js`. `MDXComponents` file is used to inject your custom components to your markdown.
```jsx
// My _app.js file
import 'styles/global.scss';

import { ThemeProvider } from 'next-themes';
import { MDXProvider } from '@mdx-js/react';
import MDXComponents from 'components/MDXComponents';

function MyApp({ Component, pageProps }) {
  return (
    <ThemeProvider attribute="class">
      <MDXProvider components={MDXComponents}>
        <Component {...pageProps} />
      </MDXProvider>
    </ThemeProvider>
  );
}

export default MyApp;
```

**STEP 3** - Now I can use `Nextimg` component in my MDX file

```jsx
// One of my article .mdx file
---
title: Tips For Using Async/Await - Write Better JavaScript!
tags: javaScript
---

<NextImg
  alt={`Tips-for-using-async-await-write-better-java-script`}
  src={'tips-for-using-async-await-write-better-java-script/banner.png'}
  // To disable placeholder feature use disablePlaceholder={true}
/>
```


## ⚡ Final Result

![img-placeholder.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1624721876104/8b5rJrTkm.gif)

Most of the time you might not even see this loading as Next.js Images are really optimised and load blazing fast. If you want to see this in action just head over to my website [milindsoorya.site](https://www.milindsoorya.site) and open any article. 

> Pro Tip : In-case you are not able to see this try a hard reload  by `ctrl`+`r`  or try throttling the website in the networks panel

