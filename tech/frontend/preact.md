# Preact

## Refs

{% embed url="https://preactjs.com/guide/v10/refs/" %}

## Dispatch event to parent

{% embed url="https://github.com/preactjs/preact-custom-element/issues/14" %}

You could dispatch an event to outside, like that:  
`this.base.parentElement.dispatchEvent('my-event', new Event())`

, and then, execute your function outside the component, adding an event listener to your custom element reference:

```text
myComponent.addEventListener('my-event', e => {
     // your code 
})
```

this is an example when you are using preact with vanilla JS.

## VideoJS Vimeo

{% embed url="https://github.com/Mobiliza/videojs-vimeo" %}

### Disable Seek

{% embed url="https://github.com/vimeo/player.js/issues/356" %}

{% embed url="https://jsfiddle.net/jybleau/zes4hbdd/" %}

#### Github issue

{% embed url="https://github.com/vimeo/player.js/issues/356" %}

The most intriguing part is when user drags the timepoint. If I stop the video it gets crazy and drop never happens.

The biggest difference between Vimeo and HTML5 videos is that one uses `seeked` while the other uses `seeking`. Not the same. Plus, using `setCurrentTime` fires again `seeked`

### Disable Seek 2

{% embed url="https://jsfiddle.net/jybleau/7Lhtcvxk/" %}

[https://github.com/vimeo/player.js/issues/61](https://github.com/vimeo/player.js/issues/61)

## VideoJS Youtube

{% embed url="https://github.com/videojs/videojs-youtube" %}

Together with `@vimeo/player`

### Progress control disable in VideoJS

{% code title="line 14981" %}
```javascript
 ProgressControl.prototype.disable = function disable() {
    this.children().forEach(function (child) {
      return child.disable && child.disable();
    });

    if (!this.enabled()) {
      return;
    }

    this.off(['mousedown', 'touchstart'], this.handleMouseDown);
    this.off(this.el_, 'mousemove', this.handleMouseMove);
    this.handleMouseUp();

    this.addClass('disabled');

    this.enabled_ = false;
  };
```
{% endcode %}

## E-signature

{% embed url="https://codepen.io/dus7/pen/qGQbVP" %}

## useCallback

_“Every callback function should be memoized to prevent useless re-rendering of child components which use the callback function”_ is the reasoning of his teammates.

{% embed url="https://dmitripavlutin.com/dont-overuse-react-usecallback/" %}



