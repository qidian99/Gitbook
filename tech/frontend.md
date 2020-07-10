# Frontend

## What is Shadow DOM

{% embed url="https://juejin.im/post/5cd3aa8f6fb9a0322a1f8eac" %}

## Virtual DOM & Shadow DOM

{% embed url="https://www.blog.duomly.com/what-is-the-difference-between-shadow-dom-and-virtual-dom/" %}

> Virtual DOM is a concept of DOM used by React.js and Vue.js. In Virtual DOM concept copy of DOM is saved in the memory and while any change is done in the DOM, it’s compared to find differences. Then browser knows which elements were changed and can update only those part of the application to avoid re-rendering all the DOM. It’s done to improve the performance of the UI libraries. As we know, form the previous paragraph in DOM, every element is re-rendered, no matter if it was changed or not. Let’s check in depth how Virtual DOM works step by step. So first, the change is done, and it’s done to the Virtual DOM, not to the original DOM, then the Virtual DOM is compared with the Document Object Model, and this process is called „diffing”. While the differences are found then browser know which elements in the original DOM should be updated and the update is done. In the Virtual DOM concept, it’s possible to apply more than one change at once, to avoid re-rendering for every single element change. The biggest issue that Virtual DOM solves is the performance improvement on DOM manipulation.



1. Compiler
   * h1 h2 h3 all blue is compiled together in Sass but separate in LESS
2. Conditionals
   1. Sass has if/then/else
   2. LESS uses "guarded mixins"
3. Additional Tools
   1. Sass: Compass
   2. LESS Elements

