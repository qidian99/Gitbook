---
description: drupalized.me会员教程
---

# Drupal如何与React集成

{% embed url="https://drupalize.me/tutorial/introduction-react-and-drupal?p=3253&from=singlemessage&isappinstalled=0" %}

## Introduction

{% file src="../.gitbook/assets/introduction-to-react-and-drupal-\_-drupalize.me.pdf" caption="1：介绍" %}

React is a JS library while Drupal is a CMS.

## React Basics

{% file src="../.gitbook/assets/react-basics-\_-drupalize.me.pdf" caption="2：React基础" %}

### Atomic Design

Top-down designs provided by page and screen. We then took those designs and broke them down into their containers, grid systems, and elements to write our markup. This is how Drupal core and most web development works.

![Drupal uses Top-Down design](../.gitbook/assets/image%20%2828%29.png)

Atomic Design, a design-systems methodology outlined by Brad Frost, states that we should start with the basic building blocks, or atoms, of a page and work our way up to create designs:

![](../.gitbook/assets/image%20%2811%29.png)

### Components

Component-based, starting from reusable UIs and build up the larger component.

> Components let you split the UI into independent, reusable pieces, and think about each piece in isolation.

![](../.gitbook/assets/image%20%2826%29.png)

> Components and their behaviors are intimately dependant and should be encapsulated like so. -- James Long on Removing User Interface Complexity or Why React is Awesome

I cannot agree more -- different behaviors and actions have their context or scope -- which is the component.

### JSX

JSX must be compiled into JavaScript in order for the browser to use it. This is typically done via a build process where tools like Babel take your JSX \(and ES6+\) code, transform it into valid JavaScript, and save the result as a production-ready asset, which is served to the browser.

### State

The behavior of a component is tracked and manipulated via _state._

State can make the component re-render \(component-wise re-rendering is achieved via Virtual DOM 我的观点\)

### One-way data flows

In React, data only flows one way: **down the component hierarchy**. To update data, we send events up the hierarchy to trigger changes to data, or state, which then flow back down to the components.

![](../.gitbook/assets/image%20%281%29.png)

React components can have 2 forms of data: **props** and **state**

* Props are properties received from ancestors in the component hierarchy, and cannot be changed, or mutated.
* State is local to a component, and can be changed by events. Child components can receive both the values of that state and events to update that state through props.

### Blazing fast speed

React is blazing fast across two dimensions:

* Loads fast due to its small size
* Re-renders quickly when responding to changes due to the virtual DOM.

| NAME | SIZE \(MINIFIED\) | SIZE \(GZIPPED\) |
| :--- | :--- | :--- |
| Vue 2.4.2 | 58.8K | 20.9K |
| React 16.2.0 + React DOM | 97.5K | 31.8K |
| Angular 1.4.5 | 143K | 51K |
| Ember 2.2.0 | 435K | 111K |
| Angular 2 | 566K | 111K |

### Virtual DOM

The virtual DOM is an ideal representation of the UI in memory. 

React syncs the real DOM with the virtual DOM through a process called **reconciliation**. 

* diffs the before and after states to only re-render the parts of the DOM that changed.
* This results in fewer wasted renders and thus higher, or faster, responsiveness. 

React also provides a lifecycle method where you can override the algorithm to state whether a component should update or not.

### Hooks

Functions that let you hook into a React application's state and lifecycle.

Added in version 16.8. They are quickly becoming the recommended way to implement state, interact with the component lifecycle, and handle other React features that previously required writing a class. 

Hooks are designed to make it easier to reuse stateful logic between components. 

* For example, 2 different components might need to be able to determine if the current user is logged in or not and adjust their display accordingly.

The biggest win you get from using hooks is that you can **write React components that are functions** instead of classes and the resulting code is often **easier to test and maintain.**

There are currently no plans to remove classes from React, and you can always write a class component, and use the standard lifecycle methods. However**, in the real-world we're seeing hooks used more and more**

### ES6+

"ES" is short for [ECMAScript](https://en.wikipedia.org/wiki/ECMAScript). The number after "ES" refers to the edition of ECMAScript. 

ECMAScript is the standard used for ongoing development. The most popular implementation of ECMAScript is JavaScript. 

**W**e generally refer to ES6 and above as ES6+ or ES2015+. The React community has embraced ES6+ with open arms. Currently, almost all tutorials \(including ours!\) and documentation are written using ES6+ syntax.

### Transpiling

Since not all of the awesome features of ES6 are available to all browsers, your ES6 code can be run through a _transpiler_ \(translator + compiler\). The most common transpiler is [Babel](https://babeljs.io/). 

**Babel converts code from one version of JavaScript to another**, and outputs a new script that even IE 11 will understand. 

There are in-browser compiler and the command-line compiler.

### Community

Unlike Drupal where popular modules and patterns are often incorporated into core, **React does not incorporate or adopt popular libraries.** They keep the scope narrow so that React itself remains nimble and easier to update. 

Development is focused on delivering better **on the core**, including a relentless pursuit of higher performance to improve user experience. 

This also means that it is more difficult to define "best practices" in React since is it more of an ecosystem rather than a monolith like Drupal that standardizes over time.

### React and the JavaScript ecosystem

Since React itself is just one small package, the ecosystem to get a full application up and running can be intimidating. From ES6, Babel, and Webpack to npm to state management, routing, and sync requests, it can be a lot to learn for newcomers.

Better tools exist now for bootstrapping new React applications that don't require you to learn how to best manage Babel and Webpack for transpiling and bundling your applications. Tools like Create React App and Next.js have been a godsend for helping new and experienced developers alike focus on React and get coding faster.

The trend is to keep improving these tools that help make developers' lives easier so that they can ship code faster.

如果你想体验Drupal Deployment from scratch的艰辛，可以看看我这个Tutorial:

{% page-ref page="chuang-jian-yi-ge-jie-ou-de-drupal-9react-ying-yong.md" %}

## Decoupled vs. Progressively Decoupled

{% file src="../.gitbook/assets/decoupled-vs.-progressively-decoupled-\_-drupalize.me.pdf" caption="3：解耦与逐步解耦" %}

React and Drupal can be used together in two different ways: fully decoupled, also known as headless; or progressively decoupled.

### Which way to use

Answer this question: _is the JavaScript that makes up your React application part of an existing Drupal page?_

* If the answer is yes, then you're working on a progressively decoupled application wherein **Drupal's theme layer and React are both responsible for what is on the page**. 
  * In this scenario, React _provides complex interactive elements_, or pulls in and displays information from _third party services_
  * while Drupal and Twig control the majority of what is presented on the page.
* If the answer is no, then you're using Drupal in a decoupled manner, wherein _Drupal acts solely as the data store for your application and plays no part in determining the user experience_. All the markup is provided by the React application.

### Decoupled

The terms _decoupled_, _headless_, and _stand-alone_ all refer to a scenario in which a **React application provides everything the user sees on the page**. That application **interacts with Drupal via a web services API to load content or authenticate users**. 

Think of this as two independent applications -- one React, one Drupal -- that communicate with one another via an API.  


![](../.gitbook/assets/image%20%2814%29.png)

_Bringing data from Drupal into React requires setting up an API that exposes Drupal content at API endpoints, and then telling React to grab that data from the endpoint and parse that data as JSON._

#### pros

1. Works well for development teams that are already well versed in React application development
2. A component-based approach to designing and composing the UI
3. Gives front-end developers complete control over the layout, look, and feel of the site.
   * For example, a site administrator can't place a block into a region and change the UX of the site
4. Front-end developers can use the tools they want to use in the way they are intended to be used rather then trying to coerce them into working within the context of a Drupal theme
5. Your React application can potentially be hosted as a simple static site using GitHub pages, S3, or various other options

### Progressively decoupled

The phrase _progressively decoupled_ generally refers to any situation where **Drupal's theme layer renders the initial state** and a JavaScript framework like **React performs further rendering on the client side.**

In this case Drupal, with a standard Drupal theme, outputs the primary user experience, and React enhances it. 

* If you've built Drupal themes before, you've probably used jQuery to provide user experience enhancements. This is the same thing, created with React instead of jQuery.

#### Pros

* You don't have to worry about doing server side rendering
* Drupal has powerful built-in tools for layout, views, displays, etc. If you need to provide editors or content creators the ability to control these things it's generally much easier to do with a traditional Drupal theme
* React code \(which is just JavaScript\) can ride along with the authentication that's already happening between your browser and Drupal

## Connect React to a Drupal Theme or Module

{% file src="../.gitbook/assets/connect-react-to-a-drupal-theme-or-module-\_-drupalize.me.pdf" caption="4：React与Drupal模块的连接" %}

There are a lot of different ways you could go about setting this all up. Do you add React via a **theme** or a **module**? Do you need a **build tool**? Should you use **Webpack**, or **Babel**, or **Parcel**, or something else? We can help you figure out what is required and you can adapt our suggestions to meet your needs.

* Create a new custom theme with the required build tools to develop React applications
* Add a DOM element for our React application to bind to
* Create a "Hello, World" React component to verify everything is working

{% embed url="https://github.com/DrupalizeMe/react-and-drupal-examples/tree/master/drupal/web/themes/react\_example\_theme" %}

### How things connect

To add React to a Drupal module or theme:

1. Use an **asset library** to add the React JavaScript library
2. Include our custom JavaScript code that uses the React library via an asset library
3. Modify the HTML of the page and add a DOM element like `<div id="react-app">` for our React application to bind to

It's possible to add the React library via a plain `<script>` tag in your HTML, like you might have included jQuery in the past. 

* However, we couldn't come up with any real-world scenarios in which you would do this to add React to a Drupal module or theme. Our example code demonstrates how to do it if you really want to know.
* We think that in most cases you'll want to set up a build toolchain for your React \(and other JavaScript\). This will allow you to include 3rd-party packages from npm, scale your application to many files and components, enable live editing in development mode, and help optimize things for speed and compatibility.

### Setup a new theme

For this example we'll [create a new theme](https://drupalize.me/tutorial/describe-your-theme-info-file) and add our JavaScript to the theme. You could also opt to do these same steps in an existing theme, or as part of a custom module.

1. In your Drupal installation, go to _/themes_.
2. Create a new theme folder called _react\_example\_theme_.
3. Create a new file named _react\_example\_theme.info.yml_ with the following contents: 
4. Enable the theme by navigating to _Appearance_ \(admin/appearance\) in _Manage_ administration menu. Then press the _Install and set as default_ link for the _React Example Theme_ theme.

```yaml
name: React Example Theme
type: theme
description: 'A theme that loads React JavaScript libraries, and a basic React application.'
core: 8.x
base theme: bartik
```

Initially, things won't look any different as all we've done is create a sub-theme of Bartik. We'll add some customizations in a moment.

### Create a custom React script

To test that everything is working we can create a "Hello, World" React component.

1. Create a _src/_ directory for your JavaScript if you haven't already, eg. _/themes/react\_example\_theme/js/src_.
2. Create a new file called _index.jsx_: _/themes/react\_example\_theme/js/src/index.jsx_.
3. Add a "Hello, World" sample React script to your _index.jsx_ file.

Add the following to the new _index.jsx_ file:

```jsx
import React from 'react';
import ReactDOM from 'react-dom';

// # Example 1: Simple "Hello, World" code
ReactDOM.render(
  <h1>Hello there - world!</h1>,
  document.getElementById('react-app')
);
```

This is the [simplest React script](https://reactjs.org/docs/try-react.html#minimal-html-template) from the React website. The `ReactDOM.render()` \(i.e. virtual dom\) will look for an HTML element with the ID _react-app_, and replace it with the HTML markup produced by your React code. This is also known as _binding_ the React application to the DOM.

