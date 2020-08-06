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

## VideoJS Youtube

{% embed url="https://github.com/videojs/videojs-youtube" %}

Together with `@vimeo/player`



