# GSAP and ScrollMagic

## Install bonus plugins after registration

{% embed url="https://greensock.com/docs/v3/Installation\#download" %}

## Import all gsap features in webpack

{% embed url="https://greensock.com/forums/topic/16676-best-way-to-use-gsap-with-webpack/" %}

Follow this post I got to

{% embed url="https://greensock.com/forums/topic/15075-using-npm-and-webpack-to-import-tweenlite-scrolltoplugin-draggable-es6/" %}

{% embed url="https://github.com/OSUblake/gsap-webpack/blob/master/gsap-webpack/gsap-webpack/webpack.config.js" %}

## GSAP webpack aliases

{% embed url="https://greensock.com/forums/topic/17233-loading-timelinelite-correctly-with-npm-webpack/" %}

```javascript
const webpack = require("webpack");
import path from 'path';
import { lstatSync, readdirSync } from 'fs';

const gsapPath = "node_modules/gsap/src/uncompressed/";
const scrollMagicPath = "node_modules/scrollmagic/scrollmagic/uncompressed/";

const isDirectory = source => lstatSync(source).isDirectory();
const getDirectories = source =>
  readdirSync(source).map(name => path.join(source, name)).filter(isDirectory);

export default (config, env, helpers) => {
  // getDirectories('src/').map((dir) => {
  //   config.resolve.alias[dir.replace('src/', '')] = path.resolve(__dirname, dir);
  // });
  // console.log(path.resolve(__dirname, 'node_modules/scrollmagic/scrollmagic/uncompressed/ScrollMagic.js'));

  const aliases = config.resolve.alias;
  aliases.react = "preact/compat";
  aliases["react-dom"] = "preact/compat";
  aliases["npm"] = 'node_modules';
  aliases["CSSPlugin"] = "gsap";
  aliases["TweenLite"] = "gsap";
  aliases["TweenMax"] = "gsap";
  aliases["TimelineLite"] = "gsap";
  aliases["TimelineMax"] = "gsap";
  aliases["ScrollMagic"] = path.resolve(__dirname, scrollMagicPath + 'ScrollMagic.js');
  aliases["animation.gsap"] = path.resolve(__dirname, scrollMagicPath + 'plugins/animation.gsap.js');
  aliases["debug.addIndicators"] = path.resolve(__dirname, scrollMagicPath + 'plugins/debug.addIndicators.js');
  aliases["Draggable"] = path.resolve(__dirname, gsapPath + "utils/Draggable.js");
  aliases["ScrollToPlugin"] = path.resolve(__dirname, gsapPath + "plugins/ScrollToPlugin.js");


  // let { plugin } = helpers.getPluginsByName(config, "UglifyJsPlugin")[0];
  // plugin.options.sourceMap = false
  config.plugins.push(new webpack.DefinePlugin({
    "process.env": {
      NODE_ENV: JSON.stringify("production")
    }
  }), new webpack.ProvidePlugin({
    jQuery: "jquery",
    $: "jquery",
    jquery: "jquery"
  }));
}

```

## GSAP: Invalid property cycle

{% embed url="https://greensock.com/forums/topic/22001-gsap-3-upgrade-issues/" %}



## Use with scrollmagic

{% embed url="https://greensock.com/scrollmagic/" %}

## ScrollMagic webpack issue

There are multiple modules with names that only differ in casing.

{% embed url="https://github.com/janpaepke/ScrollMagic/issues/717" %}

{% embed url="https://github.com/janpaepke/ScrollMagic/issues/717" %}

```javascript
// In webpack
aliases["ScrollMagic"] = path.resolve(__dirname,  'node_modules/scrollmagic/scrollmagic/uncompressed/ScrollMagic.js');
  
// In module
import ScrollMagic from "ScrollMagic";
```

## GSAP 3 and scrollmagic

### GSAP3 API

{% embed url="https://greensock.com/gsap3-features\#h2-2-simplified-api" %}

### GSAP3 Stagger

{% embed url="https://greensock.com/gsap3-features/" %}



