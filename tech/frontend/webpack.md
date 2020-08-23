# Webpack

{% embed url="https://www.valentinog.com/blog/babel/" %}

## Preact Webpack

{% embed url="https://github.com/preactjs/preact-cli/wiki/Config-Recipes\#setting-proxy-for-dev-server-using-config-directly" %}

{% embed url="https://stackoverflow.com/questions/46688116/webpack-configuration-in-preact-cli" %}

{% code title="preact.config.js" %}
```javascript
export default (config, env, helpers) => {
  const aliases = config.resolve.alias;
  aliases.react = "preact-compat";
  aliases["react-dom"] = "preact-compat";
  // let { plugin } = helpers.getPluginsByName(config, "UglifyJsPlugin")[0];
  // plugin.options.sourceMap = false
}

```
{% endcode %}

## Bunde

### Difference between different js bundles

{% embed url="https://www.davidbcalhoun.com/2014/what-is-amd-commonjs-and-umd/" %}

### multiple formats

[https://tech.trivago.com/2015/12/17/export-multiple-javascript-module-formats/](https://tech.trivago.com/2015/12/17/export-multiple-javascript-module-formats/)

