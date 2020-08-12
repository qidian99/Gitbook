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

### Dispatch one time event example

```javascript
 function getApprovers(index = 0) {
      return new Promise((resolve) => {
        const element = document.getElementsByTagName('custom-approval')[index];
        const tempListener = (event) => {
          element.removeEventListener('export-approvers', tempListener);
          console.log('get it', event.detail);
          const approvers = event.detail;
          resolve(approvers);
        };
        element.addEventListener('export-approvers', tempListener);
        element.dispatchEvent(new CustomEvent('get-approvers'));
      })
    }
```

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

## Dialog polyfill

{% embed url="https://medium.com/@mancebo128/native-html-dialog-in-angular-with-dialog-polyfill-2b99f5e2f4ae" %}

### React modal

```jsx
import ReactModal from 'react-modal';

<ReactModal

  isOpen={
    false
  /* Boolean describing if the modal should be shown or not. */}

  onAfterOpen={
    handleAfterOpenFunc
  /* Function that will be run after the modal has opened. */}

  onAfterClose={
    handleAfterCloseFunc
  /* Function that will be run after the modal has closed. */}

  onRequestClose={
    handleRequestCloseFunc
  /* Function that will be run when the modal is requested
     to be closed (either by clicking on overlay or pressing ESC).
     Note: It is not called if isOpen is changed by other means. */}

  closeTimeoutMS={
    0
  /* Number indicating the milliseconds to wait before closing
     the modal. */}

  style={
    { overlay: {}, content: {} }
  /* Object indicating styles to be used for the modal.
     It has two keys, `overlay` and `content`.
     See the `Styles` section for more details. */}

  contentLabel={
    "Example Modal"
  /* String indicating how the content container should be announced
     to screenreaders */}

  portalClassName={
    "ReactModalPortal"
  /* String className to be applied to the portal.
     See the `Styles` section for more details. */}

  overlayClassName={
    "ReactModal__Overlay"
  /* String className to be applied to the overlay.
     See the `Styles` section for more details. */}

  id={
    "some-id"
  /* String id to be applied to the content div. */}

  className={
    "ReactModal__Content"
  /* String className to be applied to the modal content.
     See the `Styles` section for more details. */}

  bodyOpenClassName={
    "ReactModal__Body--open"
  /* String className to be applied to the document.body 
     (must be a constant string).
     This attribute when set as `null` doesn't add any class 
     to document.body.
     See the `Styles` section for more details. */}

  htmlOpenClassName={
    "ReactModal__Html--open"
  /* String className to be applied to the document.html
     (must be a constant string).
     This attribute is `null` by default.
     See the `Styles` section for more details. */}

  ariaHideApp={
    true
  /* Boolean indicating if the appElement should be hidden */}

  shouldFocusAfterRender={
    true
  /* Boolean indicating if the modal should be focused after render. */}

  shouldCloseOnOverlayClick={
    true
  /* Boolean indicating if the overlay should close the modal */}

  shouldCloseOnEsc={
    true
  /* Boolean indicating if pressing the esc key should close the modal
     Note: By disabling the esc key from closing the modal
     you may introduce an accessibility issue. */}

  shouldReturnFocusAfterClose={
    true
  /* Boolean indicating if the modal should restore focus to the element
     that had focus prior to its display. */}

  role={
    "dialog"
  /* String indicating the role of the modal, allowing the 'dialog' role
     to be applied if desired.
     This attribute is `dialog` by default. */}

  parentSelector={
    () => document.body
  /* Function that will be called to get the parent element
     that the modal will be attached to. */}

  aria={
    {
      labelledby: "heading",
      describedby: "full_description"
    }
  /* Additional aria attributes (optional). */}

  data={
    { background: "green" }
  /* Additional data attributes (optional). */}

  overlayRef={
    setOverlayRef
  /* Overlay ref callback. */}

  contentRef={
    setContentRef
  /* Content ref callback. */}>
    <p>Modal Content</p>
</ReactModal>
```

## Custom Element overlapping

{% embed url="https://stackoverflow.com/questions/39809837/element-overlapping-when-absolute-positioning-applied-on-a-custom-tag-html-cs/39809954" %}

## Import fonts

{% embed url="https://dev.to/alaskaa/how-to-import-a-web-font-into-your-react-app-with-styled-components-4-1dni\#:~:text=In%20your%20React%20project%2C%20create,the%20rest%20of%20your%20app." %}

{% embed url="https://stackoverflow.com/questions/41676054/how-to-add-fonts-to-create-react-app-based-projects" %}

## SSR

{% embed url="https://github.com/preactjs/preact-cli/issues/940" %}

## CSS Transition

{% embed url="https://ozmoroz.com/2019/03/react-css-transitions/" %}



