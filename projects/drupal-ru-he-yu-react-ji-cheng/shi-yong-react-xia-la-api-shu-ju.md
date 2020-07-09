---
description: Retrieve Data from an API with React
---

# 使用React下拉API数据

{% file src="../../.gitbook/assets/retrieve-data-from-an-api-with-react-\_-drupalize.me.pdf" caption="7：使用React下拉API数据" %}

## Content versus configuration

In the context of a Drupal application there are two types of data we can use:

* **Content**: User-generated content like a list of blog posts, calendar events, or taxonomy terms.
* **Configuration**: Administrator-configurable settings that influence how our application works. For example, what content to list, or the number of items to display in the list.

When dealing with **content** the best path is to enable the Drupal core JSON API module and make API requests to Drupal, just like you would if your application was decoupled from Drupal.

Learn more about this approach in [Use React to List Content from Drupal](https://drupalize.me/tutorial/use-react-list-content-drupal).

For **configuration**, there are a couple of options. If your React code is embedded in a Drupal theme or module you can [Use Server-Side Settings with drupalSettings](https://drupalize.me/tutorial/use-server-side-settings-drupalsettings) to pass configuration data. This is by far the easiest approach. If your JavaScript code is decoupled from Drupal you'll need to find a way to expose the required configuration as JSON:API data. This could mean creating a new JSON API resource or hanging your configuration on an existing entity type like [Consumers](https://www.drupal.org/project/consumers).

## Get data from an API via HTTP requests

The most common way to make network requests with modern JavaScript is via the Fetch API, or one of the libraries that wrap `fetch` with additional features. We're discussing this in the context of a React application. But React is _just_ JavaScript, and anything we do here will work in most JavaScript applications.

### Why not jQuery?

In Drupal 8, jQuery is included with core. Its `.get` and `.ajax` methods are powerful utilities for making network requests with JavaScript. However, there are good reasons to not use jQuery for HTTP requests.

**First, jQuery is no longer loaded on every page by default.** One of the main reasons to use jQuery in your Drupal JavaScript code is the fact that it's there already. But if you don't need it you can avoid the overhead, and potential performance penalty, of loading it.

The JavaScript language has evolved significantly since jQuery was created, and now includes built-in functionality for handling asynchronous network requests in the form of [promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises). There are now more efficient ways of writing [_closures_](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures), which tidy up your JavaScript so that _you do not wind up with complicated "trees" of nested asynchronous calls._

`fetch` is easier and cleaner to deal with than the longstanding XHR \([XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)\) format of the past. Abstractions like jQuery's `.get` are no longer necessary in most cases.

### What is `fetch`?

The [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) provides a native JavaScript interface for retrieving data over a network. It's built into ES6.

The `fetch()` function lets you send network requests and get responses. It uses promises to allow for asynchronous requesting and processing of data.

* [Fetch - living standard](https://fetch.spec.whatwg.org/)
* [Introduction to fetch\(\)](https://developers.google.com/web/updates/2015/03/introduction-to-fetch)

### What are promises?

Promises are a way that you can pass an asynchronous function, and if the request is successful, do certain behaviors after the response is complete. If the function is not successful, say that it failed and do something else.

* [Using Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises) and [Promise documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
* [JavaScript promises for dummies](https://scotch.io/tutorials/javascript-promises-for-dummies)

### Anatomy of a `fetch()` request

Consider the following example JavaScript code:

```text
fetch(url, options)
  .then(response => response.json())
  .then(result => console.log('success', result))
  .catch(error => console.log('error', error));
```

Here's what's happening in this request:

1. Send the request with `fetch(url, options)` along with options, which can contain your authentication header information, timeouts, and CORS settings.
2. Get a response from the server and convert it to JSON. Note: This fails if it's a 500 error or there's a network connection failure.
3. Do something with the data. Since fetch passes a promise, you will either need to pass a method that knows how to update your code or process the results and run the interface changes you want to make _inside_ the promise.
4. Catch any errors: `.catch(error => console.log('error', error));`. This is the handling of the 500 errors from the original `fetch` request, or if something else breaks in the sequence.

**Note:** `.then` methods can take 2 functions: one for handling success and another for handling errors. Depending on what you are doing, you may need to reject your promise in a `.then`. For example, you might retrieve some data, parse the response, and notice that the response itself contains an error.

### Libraries for AJAX requests

Fetch is a standardized and well-documented native JavaScript utility for making asynchronous network requests. We will use `fetch` in these tutorials. It's not 100% supported, but close enough for our needs.

One commonly used alternative is [Axios](https://www.npmjs.com/package/axios). Axios is like `fetch` but gives you a little bit more control over your network requests. [`Superagent`](https://visionmedia.github.io/superagent/) and [`request`](https://github.com/request/request) also provide similar functions, but have Node.js packages that you can include for isomorphic \(i.e. server-side AND client-side\) React applications.

For this set of tutorials `fetch` will work just fine. But it's worth at least familiarizing yourself with the other available options.

### Using ES6 functions to iterate through data

Once you've made a request and retrieved some data, you'll need to traverse the data and do something with it. ES6 comes with functions for mapping \(`.map`\), filtering\(`.filter`\) and iterating over your data \(`.each`\). For this tutorial, we will use the `.map` feature, but the others are similar. We will go over this with `jsonapi` data.

On the Mozilla Developer Network, read more about ES6 functions you can use to iterate through date:

* [Array.prototype.map\(\)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
* [Array.prototype.filter\(\)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)
* [Array.prototype.each\(\)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/each)
* [Loops and iteration](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Loops_and_iteration)

It's also a good idea to be familiar with `arrow functions` and how they work. That syntax is commonly used when working the above array functions. Learn more about [arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions).

