# AMD

To make this easier, and to make it easy to do a simple wrapping around CommonJS modules, this form of define is supported, sometimes referred to as "simplified CommonJS wrapping":

```text
define(function (require) {
    var dependency1 = require('dependency1'),
        dependency2 = require('dependency2');

    return function () {};
});
```

The AMD loader will parse out the require\(''\) calls by using Function.prototype.toString\(\), then internally convert the above define call into this:

```text
define(['require', 'dependency1', 'dependency2'], function (require) {
    var dependency1 = require('dependency1'),
        dependency2 = require('dependency2');

    return function () {};
});
```

This allows the loader to load dependency1 and dependency2 asynchronously, execute those dependencies, then execute this function.

