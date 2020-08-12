# Use Preact to Create Web Components

{% embed url="https://preactjs.com/" %}

1. Getting started with Preact by installing the cli

   ```text
   npm install -g preact-cli
   ```

2. Initialize default template

   ```text
   preact create default my-project
   ```

   Note: this will initialize the project in the current directory \(--force\)

3. Development

   ```text
   # Go into the generated project folder
   cd my-project

   # Start a development server
   npm run dev
   ```

4. Production build

   ```text
   npm run build
   ```

## Web Components

{% embed url="https://github.com/preactjs/preact-custom-element" %}

## Build the template

{% embed url="https://github.com/preactjs/preact-cli\#preact-build" %}

{% embed url="https://github.com/preactjs/preact-custom-element/issues/28" %}

```text
$ preact build

    --src              Specify source directory  (default src)
    --dest             Specify output directory  (default build)
    --cwd              A directory to use instead of $PWD  (default .)
    --sw               Generate and attach a Service Worker  (default true)
    --json             Generate build stats for bundle analysis
    --template         Path to custom HTML template
    --preload          Adds preload tags to the document its assets  (default false)
    --analyze          Launch interactive Analyzer to inspect production bundle(s)
    --prerenderUrls    Path to pre-rendered routes config  (default prerender-urls.json)
    -c, --config       Path to custom CLI config  (default preact.config.js)
    --esm              Builds ES-2015 bundles for your code.  (default true)
    --brotli           Adds brotli redirects to the service worker.  (default false)
    --inline-css       Adds critical css to the prerendered markup.  (default true)
    -v, --verbose      Verbose output
    -h, --help         Displays this message
```

## Demo site

{% embed url="https://webcomponents.dev/edit/3DhOnddktSXAlGHR0t09" %}

## Convert it to browser 

```text
browserify BUNDLE.js -o YOUR_CUSTOM_ELEMENT.js
```

## Export bundle SH script

```bash
preact build --no-prerender --template ./src/wcs/index.js --src ./src/wcs --dest wcs
find wcs ! -name '*.esm.*' -name "bundle.*.js" -exec browserify {} -o dist/isd-wc.js \;
find wcs ! -name '*.esm.*' -name "bundle.*.css" -exec cp {} dist/isd-wc.css \;
```

## Example by Dian

### Wrap Preact components in custom element

```jsx
import register from 'preact-custom-element';
import { Component, html } from 'htm/preact';
import { useState, useCallback } from 'preact/hooks';

class DianGreeting extends Component {

  constructor() {
    super();
    this.state = { name: '' };
  }


  static getDerivedStateFromProps(props, state) {
    if (!state.name) {
      return { name: props.name }
    }
    return state;
  }

  static tagName = 'dian-greeting';

  // Track these attributes:
  static observedAttributes = ['name'];

  updateText = (e) => {
    const { name } = this.state;
    this.setState({ name: `${name}-lol` });
  };

  render() {
    const { name } = this.state;
    return (
      <div style={{ border: "1px solid black" }}>
        <p>Hello, {name}!</p>
        <button onClick={this.updateText}>Click Me</button>
      </div>);
  }
}


register(DianGreeting, DianGreeting.tagName, DianGreeting.observedAttributes);

```

### Build the template

```text
preact build --template ./src/components/greeting.js 
```

### Browserify the bundle

```text
 browserify ssr-bundle.js -o test.js
```

### Use the custom element

```markup
<!DOCTYPE html>
<html>

<head>
 <script src="test.js"></script>
</head>


<body>
  <dian-greeting name="haha"></dian-greeting>
</body>
</html>

```

![](../../.gitbook/assets/image%20%2858%29.png)

![](../../.gitbook/assets/image%20%2859%29.png)

### Commands backup

```bash
preact build --template ./src/wcs/isd-poc.jsx --src ./src/wcs --dest wcs--src ./src/wcs
browserify wcs/ssr-build/ssr-bundle.js -o dist/isd-wc.js
cp wcs/ssr-build/ssr-bundle.*.css dist/isd-wc.css
```

```text
#!/bin/sh

preact build --template ./src/wcs/isd-poc.jsx --src ./src/wcs --dest wcs--src ./src/wcs
browserify wcs/ssr-build/ssr-bundle.js -o /Users/qidian/Sites/devdesktop/drupal-7.72/sites/default/modules/ISAFE\ Direct/isd_web_components/js/isd-wc.js
cp wcs/ssr-build/ssr-bundle.*.css /Users/qidian/Sites/devdesktop/drupal-7.72/sites/default/modules/ISAFE\ Direct/isd_web_components/css/isd-wc.css
```

```bash
preact build --template ./src/wcs/index.js --src ./src/wcs --dest wcs
# find wcs ! -name '*.esm.*' -name "bundle.*.js" -exec browserify {} -o dist/isd-wc.js \;
# find wcs ! -name '*.esm.*' -name "bundle.*.css" -exec cp {} dist/isd-wc.css \;
find wcs/ssr-build -name "ssr-bundle.js" -exec browserify {} -o dist/isd-wc.js \;
find wcs/ssr-build -name "ssr-bundle.*.css" -exec cp {} dist/isd-wc.css \;
```

## Use Shadow DOM

```text
npm i -S preact-shadow-dom
```

```jsx
import ShadowDOM from "preact-shadow-dom";

var MyComponent = () => (
	<div className="my-component-class">
		<h1>My Component</h1>
	</div>
);

var ShadowMyComponent = ShadowDOM(MyComponent);


// With CSS, injected into the shadow DOM root:

import ShadowDOM from "preact-shadow-dom";
import styles from "styles.css";

var MyComponent = () => (
	<div className="my-component-class">
		<h1>My Component</h1>
	</div>
);

// styles is raw css that will be injected into the shadow dom
var ShadowMyComponent = ShadowDOM(MyComponent, styles);
```

## Use LESS

```text
 npm install --save-dev less less-loader
```

## Define allowed props value

{% embed url="https://reactjs.org/docs/typechecking-with-proptypes.html" %}

## Other links

{% embed url="https://itnext.io/handling-data-with-web-components-9e7e4a452e6e" %}



{% embed url="https://developers.google.com/web/fundamentals/web-components/best-practices" %}

## 

