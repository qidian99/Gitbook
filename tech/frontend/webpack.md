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



