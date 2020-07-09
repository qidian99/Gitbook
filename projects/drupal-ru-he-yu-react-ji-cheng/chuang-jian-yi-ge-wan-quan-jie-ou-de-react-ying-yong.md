---
description: Create a Fully Decoupled React Application
---

# 创建一个完全解耦的React应用

{% file src="../../.gitbook/assets/create-a-fully-decoupled-react-application-\_-drupalize.me.pdf" caption="11：创建一个完全解耦的React应用" %}

## Outline

* Introduction
  * Introduce differences we need to account for in a fully decoupled application
  * Provide an example of what the final project will look like
* Implementation
  * Use `create-react-app` to start a new project.
  * Migrate our components from previous tutorials that are part of a Drupal theme into the new application.
  * Run the hot-loading development server.
  * Create a production-ready build.
  * Optionally, connect the build code to Drupal via an asset library.
  * Optionally, modify the `create-react-app`'s Webpack configuration and scripts so that we can easily view the content in Drupal.

## Example code

{% embed url="https://github.com/DrupalizeMe/react-and-drupal-examples" %}

## Overview

In the previous tutorial, we built a "progressively decoupled" React application. This means we embedded the React application inside Drupal. We connected to Drupal's JSON:API and used the `same-origin` option with `fetch()` to send the user's Drupal session cookie and authenticate API requests. This allowed us to side-step some things that we'll now have to account for in our code. Most of these things are primarily related to authentication. Our previous code relied on the fact that it was running inside the scope of an existing Drupal theme or module, and could make use of existing cookie and session handling for authentication. If you are logged into Drupal, you are also logged into our React app. For a fully decoupled application, we'll have to handle authentication ourselves.

To do this we'll want to start using [OAuth to handle authentication and authorization](https://drupalize.me/tutorial/api-authentication-and-authorization). With OAuth, we're no longer dependent on the browser's somewhat opaque handling of cookies for authentication. Our code will also work in other contexts, for example, a React Native app where the code isn't executed inside a browser. The downside is we'll have to write a bunch of code to handle something the browser had been doing for us automatically.

You can use this same approach with any JavaScript front-end framework -- it's not specific to React. In fact, you can use this same approach with any decoupled application that needs to communicate via HTTP with Drupal. It doesn't even have to be JavaScript. This opens doors to all kinds of possibilities.

## Benefits of a decoupled React application

The primary benefit, as far as writing code goes, is that **it's much easier to follow best practices established by the React community**. This can go a long way towards helping find useful documentation, or even getting help from others, when working on your code base.

When you introduce a build tool to your React code, you get developer experience benefits. You can code with hot-reloading, where your JavaScript will update automatically in the browser whenever you save the file. You can use linting tools with your code editor to write cleaner code. You can use a variety of CSS preprocessing tools. You can break up your React code into files for each component, and use that to organize your application in a sensible way.

You can also leverage third party packages. When you set up a build tool, you can import packages into React from `npm`. With a progressively decoupled application, you might be able to add some JavaScript libraries to your page in the HTML headers, but this can get cumbersome. With a decoupled application and a working build tool, you can load libraries into components when you need them, and your entire application will be self-contained.

## What we're building

This tutorial will cover how to set up build tools to compile a React application and allow for development processes that more closely match what JavaScript developers are used to. We will use the create-react-app application to scaffold a new project, then port our existing code into that framework.

On the Drupal side, we'll install and configure the Simple OAuth module to allow for making authenticated requests to the JSON:API module. This will allow our decoupled React application to do things like POST, PATCH, and DELETE content in Drupal. We will need to refactor some of the HTTP request code to handle OAuth tokens.

{% file src="../../.gitbook/assets/use-create-react-app-to-start-a-decoupled-react-application-\_-drupalize.me.pdf" caption="12：创建一个完全解耦的React应用" %}

## What is `create-react-app`?

The [`create-react-app` application](https://github.com/facebook/create-react-app) makes it easier to start a React project that follows current best practices with regard to use of tools like Webpack and Babel, and organization of code within the project. Prior to `create-react-app` developers needed to manually create a lot of boilerplate configuration to get started. Now you can let `create-react-app` do it for you. The majority of what it sets up isn't required, but is considered best practice. In the past it often made learning React more confusing because it's hard to distinguish which parts of a project were React, and which parts were just other tools or organizational best practices.

What do you get? A boilerplate React project with configuration in place for Webpack and Babel, a built-in development server that supports hot reloading, a testing framework, a suggested pattern for organizing your code, and a lot more.

If you're familiar with Drupal console it's like using the `generate` commands that it provides to scaffold a module or theme.

You can also use your React framework of choice to scaffold your application -- be it create-react-app, Next.js, or something else that hasn't been invented yet.

## Scaffold a project with `create-react-app`

* In a Terminal, navigate to the directory on your machine where you would like your React application to live, e.g. `cd ~/Sites/decoupled-drupal/react`
* Ensure you have NodeJS version 6 or higher installed via `node --version`. TIP: use `nvm` to manage your node versions, e.g. `nvm install --lts; npm use --lts`. `npm` and `npx` are included when you install NodeJS.
* Run `npx create-react-app app` to create a new `create-react-app` application in a folder called `app`. This will scaffold a basic React application that includes a bunch of standard defaults for file organization and tooling related to React app development. Note the use of `npx` and not `npm`. You can use a different name for the directory than _app/_.
* `cd app`
* `yarn start` \(If you don't have `yarn` installed you can [install it](https://yarnpkg.com/lang/en/docs/install/#mac-stable), or use `npm run start` instead.\) When this is finished, a new browser window should open, running your React application. Try making a change to _app/src/App.js_. It should update the interface in the browser automatically via hot reloading.
* Optionally, run `yarn build` to create code that is ready to deploy to production.

#### Notes

* If you have trouble, the [official documentation for `create-react-app`](https://github.com/facebook/create-react-app#creating-an-app) is quite good.
* If you've been following along and already written an example application as part of a Drupal theme or module you can copy the _js/src/components_ directory, and _js/src/utils_ directory from your Drupal theme into the _src/_ directory of your new application.
* Alternatively, create an empty _src/components_ directory; this is where your custom React components will live.

## Update _src/App.js_

Edit the _src/App.js_ file created by create-react-app, and use the `NodeReadWrite` component, or your own custom component.

Example _src/App.js_:

```jsx
import React from 'react';
import './App.css';
import NodeReadWrite from './components/NodeReadWrite';

function App() {
  return (
    <NodeReadWrite />
  );
}

export default App;

```

![Screenshot of React application output showing components from previous tutorials. Content says &quot;No articles found&quot; and has a button to add a new article.](https://drupalize.me/sites/default/files/tutorials/cra-broken.png)

At this point your application should load, but may not be able to `GET` data from, or `POST` data to, Drupal. This is a result of the code being fully decoupled and no longer integrated into Drupal directly.

When making a `GET` request to Drupal you might see an error like the following, indicating that Drupal needs to be configured to support CORS \(cross-origin\) requests:

```text
Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at https://drupal.ddev.site/jsonapi/node/article?fields[node--article]=id,drupal_internal__nid,title,body&sort=-created&page[limit]=10. (Reason: CORS header 'Access-Control-Allow-Origin' missing).
```

Additionally, if you try to add a new article via a `POST` request, that will fail with an authorization error. We will need to set up authentication and modify our `fetch` requests to get this working. Our current example code assumes it has access to Drupal's session cookie via the browser. But since we're now running a fully decoupled app, from a different domain, that won't work.

## Integrate `create-react-app` code with a Drupal theme or module.

A previous version of this tutorial contained instructions on how to use a create-react-app codebase embedded in a Drupal theme. We've since decided to stop recommending this approach since it's fragile and error prone. We've updated [Connect React to a Drupal Theme or Module](https://drupalize.me/tutorial/connect-react-drupal-theme-or-module) with information about how to configure build tools like Webpack in the context of a Drupal theme or module.

If you're still interested in using create-react-app in this way, check out the example code in [this GitHub project](https://github.com/twfahey1/drupal-react-components). The use of `hook_library_info_alter()` seems to be the most robust way to go about including create-react-app generated bundles as Drupal asset libraries.

