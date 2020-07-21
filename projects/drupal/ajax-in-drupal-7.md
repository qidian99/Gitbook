# AJAX in Drupal 7

## Create AJAX request on the fly

{% embed url="https://www.drupal.org/docs/7/api/javascript-api/creating-custom-drupalajax-object-on-the-fly-and-attach-it-to-any-dom" %}

### For randomizing the ajax base ID

{% embed url="https://drupal.stackexchange.com/questions/86923/how-to-add-confirmation-to-ajax-link" %}

```javascript
(function($) {

Drupal.behaviors.tomsAjaxLinks = {
  attach: function(context, settings) {
    $('.toms-ajax',context).once('toms-ajax').on('click', this.handleAjax);
  },
  handleAjax: function(e) {
    // Cache the anchor link
    var $element = $(this);

    // We need some unique id, either ID of link or create our own
    var nowStamp = new Date().getTime() + Math.floor(Math.random() * (1000 - 0) + 0);
    var base = $element.attr('id') || 'toms-ajax-'+ nowStamp;

    // Change the event type to load, so we can trigger it ourselves
    var drupal_ajax_settings = {
      url : $element.attr('href'),
      event : 'load',
      progress : {
        type: 'throbber',
        message : '',
      }
    };

    // Create the ajax object
    Drupal.ajax[base] = new Drupal.ajax(base, this, drupal_ajax_settings);

    // Your confirmation code e.g. Jquery UI Dialog or something
    // Open dialog
    if(yes) {
      $element.trigger('load');
    } else {
      // Dont trigger ajax
    }
  }
}; 

})(jQuery);
```

## AJAX Commands in Form API

{% embed url="http://www.jaypan.com/tutorial/calling-function-after-ajax-event-drupal-7" %}



