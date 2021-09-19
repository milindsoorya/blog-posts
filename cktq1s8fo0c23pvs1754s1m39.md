## CSS to ignore parent padding in 4 lines

## Introduction

CSS is damn hard. It's these small problems that will take up your entire day and energy and the will to live. And one such notorious problem has been to make a child element break out of their parent's padding. 

## The problem

I recently tried to add a newsletter to my [website](https://milindsoorya.site/) (subscribe - shameless plug) and I wanted the form to extend to the side of the browser window, but there is a parent `div` with padding for getting that uniform look throughout the site.

how it looks with parent's padding -

![1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632028001976/PTE035EmF.png)

how I wanted it to look - 


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632028024284/oyDS8NuV0.png)

this happens because the section containing newsletter is within the main div -

![3.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632028138841/3LH7liH-p.png)


Yeah I know, It sound's really simple and there are many janky solutions out there to fix it too. (The solution down below is more or less jank, but in relative terms, I believe it is really elegant 😁).

## Solution

Give this below piece of CSS to the element you wish to break out(pun Intended).
```css
.full-width {
  width: 100vw;
  margin-left: 50%;
  transform: translateX(-50%);
  max-width: 100%;
}
```
The `max-width` is required because otherwise, you may find a horizontal scrollbar.

yeah, It's that simple.


UPDATE:
As I wanted the newsletter on every page I decided to move the section div one level up, so to the same level as the main. Hence, I don't have to do this hack.