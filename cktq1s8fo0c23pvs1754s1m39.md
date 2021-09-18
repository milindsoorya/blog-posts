## CSS to ignore parent padding in 3 lines

## Introduction

CSS is damn hard. It's these small problems that will take up your entire day and energy and the will to live. And one such notorious problem has been to make a child element break out of their parent's padding. 

## The problem

I recently tried to add a newsletter to my [website](https://milindsoorya.site/) (subscribe - shameless plug) and I wanted the form to extend to the side of the browser window, but there e is a parent `div` with padding for getting that uniform look throughout the site.

Yeah I know, It sound's really simple and there are many janky solutions out there to fix it too. (The solution down below is more or less jank, but in relative terms, I believe it is really elegant 😁).

## Solution

Give this below piece of CSS to the element you wish to break out(pun Intended).
```css
.full-width {
  width: 100vw;
  margin-left: 50%;
  transform: translateX(-50%);
}
```
yeah, It's that simple.