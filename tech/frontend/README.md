# Frontend

## What is Shadow DOM

{% embed url="https://juejin.im/post/5cd3aa8f6fb9a0322a1f8eac" %}

## Virtual DOM & Shadow DOM

{% embed url="https://www.blog.duomly.com/what-is-the-difference-between-shadow-dom-and-virtual-dom/" %}

> Virtual DOM is a concept of DOM used by React.js and Vue.js. In Virtual DOM concept copy of DOM is saved in the memory and while any change is done in the DOM, it’s compared to find differences. Then browser knows which elements were changed and can update only those part of the application to avoid re-rendering all the DOM. It’s done to improve the performance of the UI libraries. As we know, form the previous paragraph in DOM, every element is re-rendered, no matter if it was changed or not. Let’s check in depth how Virtual DOM works step by step. So first, the change is done, and it’s done to the Virtual DOM, not to the original DOM, then the Virtual DOM is compared with the Document Object Model, and this process is called „diffing”. While the differences are found then browser know which elements in the original DOM should be updated and the update is done. In the Virtual DOM concept, it’s possible to apply more than one change at once, to avoid re-rendering for every single element change. The biggest issue that Virtual DOM solves is the performance improvement on DOM manipulation.



1. Compiler
   * h1 h2 h3 all blue is compiled together in Sass but separate in LESS
2. Conditionals
   1. Sass has if/then/else
   2. LESS uses "guarded mixins"
3. Additional Tools
   1. Sass: Compass
   2. LESS Elements

{% embed url="http://compass-style.org/install/" %}

```text
directory testcss/ 
directory testcss/sass/ 
directory testcss/stylesheets/ 
   create testcss/config.rb 
   create testcss/sass/screen.scss 
   create testcss/sass/print.scss 
   create testcss/sass/ie.scss 
    write testcss/stylesheets/ie.css
    write testcss/stylesheets/print.css
    write testcss/stylesheets/screen.css

*********************************************************************
Congratulations! Your compass project has been created.

You may now add and edit sass stylesheets in the sass subdirectory of your project.

Sass files beginning with an underscore are called partials and won't be
compiled to CSS, but they can be imported into other sass stylesheets.

You can configure your project by editing the config.rb configuration file.

You must compile your sass stylesheets into CSS when they change.
This can be done in one of the following ways:
  1. To compile on demand:
     compass compile [path/to/project]
  2. To monitor your project for changes and automatically recompile:
     compass watch [path/to/project]

More Resources:
  * Website: http://compass-style.org/
  * Sass: http://sass-lang.com
  * Community: http://groups.google.com/group/compass-users/


To import your new stylesheets add the following lines of HTML (or equivalent) to your webpage:
<head>
  <link href="/stylesheets/screen.css" media="screen, projection" rel="stylesheet" type="text/css" />
  <link href="/stylesheets/print.css" media="print" rel="stylesheet" type="text/css" />
  <!--[if IE]>
      <link href="/stylesheets/ie.css" media="screen, projection" rel="stylesheet" type="text/css" />
  <![endif]-->
</head>
```

![Custom Compass Configuration](../../.gitbook/assets/image%20%2843%29.png)

```bash
$ gem install compass
$ cd <myproject>
$ compass install compass --sass-dir "scss" --css-dir "css" --javascripts-dir "js" --images-dir "images"
```

![](../../.gitbook/assets/image%20%2853%29.png)

```bash
compass create --sass-dir "scss" --css-dir "css" --javascripts-dir "js" --images-dir "images"
```

### Sass

#### Interpolation 

* `#{$VAR_NAME}`

![](../../.gitbook/assets/image%20%2846%29.png)

#### mixins

![Definition of a mixin](../../.gitbook/assets/image%20%2849%29.png)

![Include the mixin](../../.gitbook/assets/image%20%2845%29.png)

#### extend

![](../../.gitbook/assets/image%20%2847%29.png)

#### partials and imports

1. Create the \_base.scss file

![](../../.gitbook/assets/image%20%2848%29.png)

2. Copy the previous scss in master file into the base file, and import the base file in master file

Remember to first import the base file and then other files in the master file

![You can use the variables declared in the base file](../../.gitbook/assets/image%20%2842%29.png)

3. In this way we can separate regions \(e.g., typology vs content\)

### Compass

#### Commands

1. Sass: `sass watch test.scss:test.css`
2. `compass watch [path/to/project]`

#### Config.rb

![](../../.gitbook/assets/image%20%2844%29.png)

### Other tools to use with Sass

* Blueprint
* SUSY
* Zen \(Written by Drupal Core contributor\)
* Bourbon
