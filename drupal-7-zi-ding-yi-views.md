# Drupal 7自定义Views

{% embed url="https://hub.packtpub.com/creating-views-3-programmatically/" %}

## Declare hook\_views\_api

1. Declare the path for hook\_views\_default\_views



## Create hook\_views\_default\_views

This hook should be placed in **MODULENAME.views\_default.inc** and **it will be auto-loaded.** MODULENAME.views\_default.inc must be in the directory specified by the 'path' key returned by MODULENAME\_views\_api\(\), or the same directory as the .module file, if 'path' is unspecified.

{% embed url="https://api.drupal.org/api/views/views.api.php/function/hook\_views\_default\_views/7.x-3.x" %}

## How it works

The module we have just created could have many other features associated with it, beyond simply a view, and enabling the module will make those features and the view available, while disabling it will hide those same features and view.

When compiling the list of installed modules, Drupal looks first in its own modules directory for .info files, and then in the site’s modules directories. As can be deduced from the fact that we put our .info file in a second-level directory of sites/all/modules and it was found there, Drupal will traverse the modules directory tree looking for .info files.

We created a .info file that provided Drupal with the name and description of our module, its version, the version of Drupal it is meant to work with, and a list of files used by the module, in our case just one.

We saved the .info file as d7vrpv.info \(Drupal 7 Views Recipes programmatic view\); the name of the directory in which the module files appear \(d7vr\) has no bearing on the module itself.

The module file contains the code that will be executed, at least initially. Drupal does not “call” the module code in an active way. Instead, there are **events** that occur during Drupal’s creation of a page, and modules can elect to register with Drupal to be notiﬁ ed of such events when they occur, so that the module can provide the code to be executed at that time; for example, you registering with a business to receive an e-mail in the event of a sale. Just like you are free to act or not, but the sales go on regardless, so too Drupal continues whether or not the module decides to do something when given the chance.

Our module ‘hooks’ the views\_api and views\_default\_views events in order to establish the fact that we do have a view to offer. The latter hook instructs the Views module which function in our code executes our view: d7vrpv\_list\_all\_nodes\(\). The first thing it does is create a view object by calling a function provided by the Views module. Having instantiated the new object, we then proceed to provide the information it needs, such as the name of the view, its description, and all the information that we would have selected through the Views UI had we used it. As we are specifying the view options in the code, we need to provide the information that is needed by each **handler** of the view functionality.

The net effect of the code is that when we have cleared cache and enabled our module, Drupal then includes it in its list of modules to poll during events. When we navigate to the **Views Admin** page, an event occurs in which any module wishing to include a view in the list on the admin screen does so, including ours. One of the things our module does is deﬁ ne a path for the page display of our view, which is then used to establish a callback. When that path, list-all-nodes, is requested, it results in the function in our module being invoked, which in turn provides all the information necessary for our view to be rendered and presented.

## Handler

What is a handler? It is simply a script that performs a special task on one or more elements. Think of a house being built. The person who comes in to tape, mud, and sand the wallboard is a handler.

## To scan the all the views under a directory

```php
/**
* Implementation of hook_views_default_views().
**/
function my_great_module_views_default_views() {
  $files = file_scan_directory(drupal_get_path('module', 'essay_cloning'). '/views', '/.*\.view$/');
  foreach ($files as $filepath => $file) {
    require $filepath;
    if (isset($view)) {
      $views[$view->name] = $view;
    }
  }
  return $views;
}
```

