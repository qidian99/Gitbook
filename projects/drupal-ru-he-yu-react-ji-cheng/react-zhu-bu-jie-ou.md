---
description: Connect React to a Drupal Theme or Module
---

# React逐步解耦

{% file src="../../.gitbook/assets/connect-react-to-a-drupal-theme-or-module-\_-drupalize.me.pdf" caption="4：React与Drupal模块的连接" %}

There are a lot of different ways you could go about setting this all up. Do you add React via a **theme** or a **module**? Do you need a **build tool**? Should you use **Webpack**, or **Babel**, or **Parcel**, or something else? We can help you figure out what is required and you can adapt our suggestions to meet your needs.

* Create a new custom theme with the required build tools to develop React applications
* Add a DOM element for our React application to bind to
* Create a "Hello, World" React component to verify everything is working

{% embed url="https://github.com/DrupalizeMe/react-and-drupal-examples/tree/master/drupal/web/themes/react\_example\_theme" %}

## How things connect

To add React to a Drupal module or theme:

1. Use an **asset library** to add the React JavaScript library
2. Include our custom JavaScript code that uses the React library via an asset library
3. Modify the HTML of the page and add a DOM element like `<div id="react-app">` for our React application to bind to

It's possible to add the React library via a plain `<script>` tag in your HTML, like you might have included jQuery in the past. 

* However, we couldn't come up with any real-world scenarios in which you would do this to add React to a Drupal module or theme. Our example code demonstrates how to do it if you really want to know.
* We think that in most cases you'll want to set up a build toolchain for your React \(and other JavaScript\). This will allow you to include 3rd-party packages from npm, scale your application to many files and components, enable live editing in development mode, and help optimize things for speed and compatibility.

## Setup a new theme

For this example we'll [create a new theme](https://drupalize.me/tutorial/describe-your-theme-info-file) and add our JavaScript to the theme. You could also opt to do these same steps in an existing theme, or as part of a custom module.

1. In your Drupal installation, go to _/themes_.
2. Create a new theme folder called _react\_example\_theme_.
3. Create a new file named _react\_example\_theme.info.yml_ with the following contents: 
4. Enable the theme by navigating to _Appearance_ \(admin/appearance\) in _Manage_ administration menu. Then press the _Install and set as default_ link for the _React Example Theme_ theme.

```yaml
name: React Example Theme
type: theme
core: 8.x
description: 'A theme that loads React JavaScript libraries, and a basic React application.'
base theme: bartik
```

Initially, things won't look any different as all we've done is create a sub-theme of Bartik. We'll add some customizations in a moment.



{% embed url="https://www.drupal.org/forum/support/installing-drupal/2019-04-10/need-help-with-error-the-website-encountered-an" %}

**Note**: Drupal 9 sub-theming gives me the message above, so I switched back to Drupal 8

## Create a custom React script

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



## Set up a JavaScript toolchain with Webpack

There are a lot of possible tools you can use to do this. We'll provide an example that uses [Webpack](https://webpack.js.org/). But you should be able to setup a toolchain using Parcel, Gulp, or your tool of choice following a similar recipe.

At a high-level what we're doing is configuring a process that'll take our source JavaScript files, like _index.jsx_, and pass them through different build steps that will ultimately output a single, optimized, _.js_ file. Using this build step allows us to take advantage of the entire React/JavaScript ecosystem while working on our Drupal module or theme.

The basic steps are:

1. Set up a toolchain that'll process your JavaScript assets into one or more "bundled" JavaScript files with a known location that doesn't change
2. Create a Drupal asset library that points to the bundled assets from your build toolchain

Almost any JavaScript build tool chain will require the use of [Node.js](https://nodejs.org/) and [npm](https://npmjs.org/). We'll assume you've got those installed already and are comfortable using them. If not, check out [Install Node.js Locally with Node Version Manager](https://heynode.com/tutorial/install-nodejs-locally-nvm).

For this example we're going to use Webpack to execute [Babel](https://babeljs.io/) on our source files and save the resulting bundled assets. To do this we'll:

* Install React, Webpack, Babel and other required npm packages
* Configure Webpack
* Configure Babel
* Define a Drupal asset library to include the compiled JavaScript assets
* Add some helper scripts to our _package.json_ to make development easier

The following setup assumes that your source JavaScript files are going to live in the _react\_example\_theme/js/src_ directory, and the entry point for your JavaScript code will be _react\_example\_theme/js/src/index.jsx_. Which we created above.

## Set up a JavaScript toolchain with Webpack

There are a lot of possible tools you can use to do this. We'll provide an example that uses [Webpack](https://webpack.js.org/). But you should be able to setup a toolchain using Parcel, Gulp, or your tool of choice following a similar recipe.

At a high-level what we're doing is ****, like _index.jsx_, and pass them through different build steps that will ultimately output a single, optimized, _.js_ file. Using this build step allows us to take advantage of the entire React/JavaScript ecosystem while working on our Drupal module or theme.

The basic steps are:

1. Set up a toolchain that'll process your JavaScript assets into one or more "bundled" JavaScript files with a known location that doesn't change
2. Create a Drupal asset library that points to the bundled assets from your build toolchain

Almost any JavaScript build tool chain will require the use of [Node.js](https://nodejs.org/) and [npm](https://npmjs.org/). We'll assume you've got those installed already and are comfortable using them. If not, check out [Install Node.js Locally with Node Version Manager](https://heynode.com/tutorial/install-nodejs-locally-nvm).

For this example we're going to use Webpack to execute [Babel](https://babeljs.io/) on our source files and save the resulting bundled assets. To do this we'll:

* Install React, Webpack, Babel and other required npm packages
* Configure Webpack
* Configure Babel
* Define a Drupal asset library to include the compiled JavaScript assets
* Add some helper scripts to our _package.json_ to make development easier

The following setup assumes that your source JavaScript files are going to live in the _react\_example\_theme/js/src_ directory, and the entry point for your JavaScript code will be _react\_example\_theme/js/src/index.jsx_. Which we created above.

### Install React, Webpack, and Babel

In your terminal run the following commands from the root directory of your theme, _themes/react\_example\_theme/_:

```bash
# Create a package.json if you don't have one already.
npm init -y
# Install the required dependencies
npm install --save react react-dom
npm install --save-dev @babel/core @babel/preset-env @babel/preset-react babel-loader webpack webpack-cli
```

### Configure Webpack with a _webpack.config.js_ file:

Create a _webpack.config.js_ file in the root of your theme.

_themes/react\_example\_theme/webpack.config.js_:



```jsx
const path = require('path');
const isDevMode = process.env.NODE_ENV !== 'production';

const config = {
  entry: {
    main: ["./js/src/index.jsx"]
  },
  devtool: (isDevMode) ? 'source-map' : false,
  mode: (isDevMode) ? 'development' : 'production',
  output: {
    path: isDevMode ? path.resolve(__dirname, "js/dist_dev") : path.resolve(__dirname, "js/dist"),
    filename: '[name].min.js'
  },
  resolve: {
    extensions: ['.js', '.jsx'],
  },
  module: {
    rules: [
      {
        test: /\.jsx?$/,
        loader: 'babel-loader',
        exclude: /node_modules/,
        include: path.join(__dirname, 'js/src'),
      }
    ],
  },
};

module.exports = config;
```

This _webpack.config.js_ uses the `isDevMode` variable to modify the configuration depending on whether you're running in "development" mode or "production" mode. When running in "development" mode we want to include source maps, and maybe other debugging information, in our builds. But we do not want those files to end up getting committed to the repository. 

* For "development" mode we change the output directory where the compiled files get saved to _js/dist\_dev_. Then we add that directory to our _.gitignore_ file to ensure development assets are never committed. This isn't required, but it's a good idea we picked up from [this post by Sam Mortenson](https://thinkshout.com/blog/2019/10/adding-webpack-to-a-traditional-drupal-theme/).

### Configure Babel with a _.babelrc_ file

Provide some configuration for Babel by creating an _.babelrc_ file with the following content in the root directory of the theme.

_themes/react\_theme\_example/.babelrc_:

```javascript
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react"
  ],
}
```

### Define a Drupal asset library

Next we need to define two new Drupal asset libraries that can tell Drupal where to find our JavaScript files. Create a _react\_example\_theme/react\_example\_theme.libraries.yml_ file, and add the following:

```text
react_app:
  version: VERSION
  js:
    js/dist/main.min.js: {minified: true}

react_app_dev:
  version: VERSION
  js:
    js/dist_dev/main.min.js: {minified: true}
```

This adds two new asset library definitions which point to the _.js_ files that are created as a result of our Webpack toolchain. Note that if you've chosen to not use the _js/dist\_dev_ trick you only need to include the first asset library definition here.

### Automatically swap asset libraries in development environments

If you are using the _js/dist\_dev_ trick you'll also need to add the following to your theme's _{THEMENAME}.theme_ file so that Drupal will switch between the production and development JavaScript assets when running locally. Learn more in [Add Logic with THEMENAME.theme](https://drupalize.me/tutorial/add-logic-themenametheme).

Create _react\_example\_theme/react\_example\_theme.theme_:

```php
<?php

/**
 * Implements hook_page_attachments_alter().
 */
function react_example_theme_page_attachments_alter(array &$attachments) {
  // Use the dev library if we're developing locally.
  if (in_array('react_example_theme/react_app', $attachments['#attached']['library']) && file_exists(__DIR__ . '/js/dist_dev')) {
    $index = array_search('react_example_theme/react_app', $attachments['#attached']['library']);
    $attachments['#attached']['library'][$index] = 'react_example_theme/react_app_dev';
  }
}
```

This code dynamically replaces all uses of the _react\_app_ asset library with the _react\_app\_dev_ asset library at runtime if the _js/dist\_dev_ directory exists.

### Tell Git to ignore development files

Update your project's _.gitignore_ file to exclude _themes/react\_example\_theme/js/dist\_dev_.

### Add some helper scripts

Let's add some helper scripts to our _package.json_ to make it easier to launch Webpack:

```javascript
"scripts": {
  "build": "NODE_ENV=production webpack",
  "build:dev": "webpack",
  "start": "webpack --watch"
}
```

Now, from the root directory of our theme, _themes/react\_example\_theme/_, we can run the following commands:

* `npm run build`: Build a production-ready set of JavaScript assets and save them into the _js/dist_ directory. Do this whenever you're ready to deploy your changes, and then commit the updated files to Git. Or, include the execution of this command in your CI build pipeline.
* `npm run build:dev`: Build a development copy \(including source maps\) of the JavaScript assets and save them into the _js/dist\_dev_ directory.
* `npm run start`: Start Webpack using `--watch` which will cause it to listen for changes to any of the files in _js/src_ and automatically rebuild the assets in _js/dist\_dev_ as needed. Useful when doing development.

Here's what your final _package.json_ should look like if you followed the steps above. It may contain other content depending on your specific use-case.

```javascript
{
  "name": "react_example_theme",
  "version": "1.0.0",
  "description": "",
  "main": "js/src/index.jsx",
  "scripts": {
    "build": "NODE_ENV=production webpack",
    "build:dev": "webpack",
    "start": "webpack --watch"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "@babel/core": "^7.8.4",
    "@babel/preset-env": "^7.8.4",
    "@babel/preset-react": "^7.8.3",
    "babel-loader": "^8.0.6",
    "webpack": "^4.41.6",
    "webpack-cli": "^3.3.11"
  },
  "dependencies": {
    "react": "^16.12.0",
    "react-dom": "^16.12.0"
  }
}
```

### Add any other Webpack configuration

Webpack can do so much more than compiling JavaScript files, and your toolchain is by no means limited to what we've configured above. For example, we could add compiling Sass to CSS, optimizing images, and allowing type-checking with TypeScript.

Check out the [Webpack Guides](https://webpack.js.org/guides/) for some examples.

**Additional examples:**

* [Adding Webpack to a traditional Drupal theme](https://thinkshout.com/blog/2019/10/adding-webpack-to-a-traditional-drupal-theme/) \(thinkshout.com\)
* [A Recipe for an Embedded React Component in Drupal](https://www.mediacurrent.com/blog/recipe-embedded-react-component-drupal) \(mediacurrent.com\)
* [Using Webpack to Unleash Your Drupal 8 Project with Modern JavaScript](https://shinesolutions.com/2018/11/05/using-webpack-to-unleash-your-drupal-8-project-with-modern-javascript/) \(shinesolutions.com\)

After following these steps you're all set to have Babel transpile your JavaScript so you can use JSX and ES6+ features in your code_. Webpack will bundle your custom code along with the required React, and ReactDOM libraries, as well as any other libraries you include using npm, into **a single JavaScript bundle**_**.**

To start developing, run the `npm run start` command. A any changes will be automatically compiled, and you can refresh the page to see the result. When you're ready to deploy your changes run `npm run build` and commit the resulting files in the _js/dist/_ directory of your theme.

## Include the asset library on one or more pages

You should now have an asset library named `react_example_theme/react_app`. **We did it in Step 4 previously.**

The next steps are to attach the new asset library to one or more pages, and add a DOM element for your React application to bind to.

In our example we'll [override](https://drupalize.me/tutorial/override-template-file) the _page.html.twig_ template to add a `<div>` into the sidebar. Our React application will render in the sidebar above any configured blocks.

Copy _core/themes/bartik/templates/page.html.twig_ into _themes/react\_example\_theme/templates_ directory and [clear the cache](https://drupalize.me/tutorial/clear-drupals-cache). \(Navigate to _Configuration_ &gt; _Performance_ \(_admin/config/development/performance_\) and select **Clear all caches**.\)

In _themes/react\_example\_theme/templates_, find this section, which renders the right column sidebar:

```php
{% if page.sidebar_first %}
  <div id="sidebar-first" class="column sidebar">
    <aside class="section" role="complementary">
      {{ page.sidebar_first }}
    </aside>
  </div>
{% endif %}
```

And replace it with this, which has the `if page.sidebar_first` conditional removed so that the sidebar is always rendered, and adds a `<div id="react-app"></div>`:

```php
<div id="sidebar-first" class="column sidebar">
  <aside class="section" role="complementary">
    <div id="react-app" class="block">React app will load here.</div>
    {{ page.sidebar_first }}
  </aside>
</div>
```

Then, include the asset library by editing the _react\_theme\_example.info.yml_ file to include the following:

```text
libraries:
  - react_example_theme/react_app
```

Finally, [clear the cache](https://drupalize.me/tutorial/clear-drupals-cache).

This will ensure that the `react_example_theme/react_app` asset library loads on every page that uses the react\_example\_theme theme.

**Learn more about:**

* Overriding template files in [Override a Template File](https://drupalize.me/tutorial/override-template-file)
* Attaching asset libraries to the page in [Attach a Library](https://drupalize.me/tutorial/attach-library)

Another approach would be to define a new Block plugin that outputs the DOM element to bind to, and attach the React application asset library to that block. Then whenever the block appears on the page the React application will load.

Example _modules/react\_example/src/Plugin/Block/ReactExampleBlock.php_:

```php
<?php
namespace Drupal\react_example\Plugin\Block;

use Drupal\Core\Block\BlockBase;

/**
 * Provides a 'ReactExampleBlock' block.
 *
 * @Block(
 *  id = "react_example_block",
 *  admin_label = @Translation("React example block"),
 * )
 */
class ReactExampleBlock extends BlockBase {

  /**
   * {@inheritdoc}
   */
  public function build() {
    $build = [];
    $build['react_example_block'] = [
      '#markup' => '<div id="react-app"></div>',
      '#attached' => [
        'library' => 'react_example/react_app'
      ],
    ];
    return $build;
  }
}
```

**Learn more about:**

* Creating Block plugins in [Implement a Plugin of Any Type](https://drupalize.me/tutorial/implement-plugin-any-type)
* Attaching asset libraries to the page in [Attach a Library](https://drupalize.me/tutorial/attach-library)

## And finally, confirm that it's working

After making all the changes above you'll have created, or modified these files either directly or by running the build toolchain:

```text
.
+-- js
|   +-- dist
|   |   +-- main.min.js
|   |   +-- main.min.js.map
|   +-- dist_dev
|   |   +-- main.min.js
|   |   +-- main.min.js.map
|   +-- src
|       +-- index.jsx
+-- node_modules/
+-- package-lock.json
+-- package.json
+-- react_example_theme.info.yml
+-- react_example_theme.libraries.yml
+-- react_example_theme.theme
+-- templates
|   +-- page.html.twig
+-- webpack.config.js
```

With the changes to our theme, or the addition of a new block, we should be able to see our React application load on the page and confirm this is all working. Load any page on your site, and you should now see the text "Hello there - world!" from our React component rendered at the top of the sidebar.

![Screenshot of Drupal page with &quot;Hello there - world!&quot; content rendered by React highlighted](https://drupalize.me/sites/default/files/tutorials/react-tutorials-hello-world.png)

Edit your JavaScript file and refresh. If you are not seeing your changes, make sure your browser is not caching your JavaScript files and clear your Drupal cache. In Google Chrome, you can go to the Dev Tools: Network Tab &gt; Disable cache.

![Disable cache highlighted in Google Chrome](https://drupalize.me/sites/default/files/tutorials/01-02-01.png)

#### Result

![](../../.gitbook/assets/image%20%2836%29.png)

