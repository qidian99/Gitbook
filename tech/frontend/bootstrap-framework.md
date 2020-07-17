# Bootstrap框架

Bootstrap, the world’s most popular front-end open source toolkit, featuring Sass variables and mixins, responsive grid system, extensive prebuilt components, and powerful JavaScript plugins

## Question

1. What's the role of Sass in this case?
2. How to use javascript in Bootstrap?
   * 到底哪些组件用到了jQuery，JS，和Popper.js?
3. Alerts for dismissing
4. Buttons for toggling states and checkbox/radio functionality
5. Carousel for all slide behaviors, controls, and indicators
6. Collapse for toggling visibility of content
7. Dropdowns for displaying and positioning \(also requires Popper.js\)
8. Modals for displaying, positioning, and scroll behavior
9. Navbar for extending our Collapse plugin to implement responsive behavior
10. Tooltips and popovers for displaying and positioning \(also requires Popper.js\)
11. Scrollspy for scroll behavior and navigation updates

## Quick Start

1. 通过CDN导入CSS
2. 直接在html里插入以下css就行

   ```markup
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css" integrity="sha384-9aIt2nRpC12Uk9gS9baDl411NQApFmC26EwAOH8WgZl5MYYxFfc+NcPb1dKGj7Sk" crossorigin="anonymous">
   ```

3. 在Body的最后插入JS

   ```markup
    <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/js/bootstrap.min.js" integrity="sha384-OgVRvuATP1z7JjHLkuOU7Xw704+h835Lr+6QL9UvYjZE3Ipu6Tp75j7Bh/kR0JKI" crossorigin="anonymous"></script>
   ```

## 开始的HTML模板

拷贝到新文件：

```markup
<!doctype html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css" integrity="sha384-9aIt2nRpC12Uk9gS9baDl411NQApFmC26EwAOH8WgZl5MYYxFfc+NcPb1dKGj7Sk" crossorigin="anonymous">

    <title>Hello, world!</title>
  </head>
  <body>
    <h1>Hello, world!</h1>

    <!-- Optional JavaScript -->
    <!-- jQuery first, then Popper.js, then Bootstrap JS -->
    <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/js/bootstrap.min.js" integrity="sha384-OgVRvuATP1z7JjHLkuOU7Xw704+h835Lr+6QL9UvYjZE3Ipu6Tp75j7Bh/kR0JKI" crossorigin="anonymous"></script>
  </body>
</html>
```

## 全局重要的地方

### HTML5文档类型

一定要加这一段

```markup
<!doctype html>
<html lang="en">
  ...
</html>
```

### Responsive meta tag

为了保证在所有设备上能有正确的渲染和点触放大，需要在meta标签中加入如下

```markup
<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
```

### Box-sizing

他们把全局的box-sizing从content-box改到了border-box。这样可以保证padding不应惜那个最后的计算出来的元素宽度

[https://developer.mozilla.org/en-US/docs/Web/CSS/box-sizing](https://developer.mozilla.org/en-US/docs/Web/CSS/box-sizing)

* content-box gives you the default CSS box-sizing behavior. If you set an element's width to 100 pixels, then the element's content box will be 100 pixels wide, and the width of any border or padding will be added to the final rendered width, making the element wider than 100px.
* border-box tells the browser to account for any border and padding in the values you specify for an element's width and height. If you set an element's width to 100 pixels, that 100 pixels will include any border or padding you added, and the content box will shrink to absorb that extra width. This typically makes it much easier to size elements.

content-box是边框不计入总长度，border-box是计入

### Sass

1. 可以改写默认的变量值
2. Every Sass variable in Bootstrap 4 includes the !default flag allowing you to override the variable’s default value in your own Sass without modifying Bootstrap’s source code. Copy and paste variables as needed, modify their values, and remove the !default flag. If a variable has already been assigned, then it won’t be re-assigned by the default values in Bootstrap.
3. complete list of Bootstrap’s variables in `scss/_variables.scss`.
4. 如何override:

   ```css
   // Your variable overrides
   $body-bg: #000;
   $body-color: #111;

   // Bootstrap and its default variables
   @import "../node_modules/bootstrap/scss/bootstrap";
   ```

5. 关于Sass映射
   * Modify map
     * To modify an existing color in our $theme-colors map, add the following to your custom Sass file:

       ```css
         $theme-colors: (
           "primary": #0074d9,
           "danger": #ff4136
         );
       ```
   * Add to map
     * To add a new color to $theme-colors, add the new key and value:

       ```css
       $theme-colors: (
         "custom-color": #900
       );
       ```

* Remove from map

  * To remove colors from $theme-colors, or any other map, use map-remove. Be aware you must insert it between our requirements and options:

    ```css
    // Required
    @import "../node_modules/bootstrap/scss/functions";
    @import "../node_modules/bootstrap/scss/variables";
    @import "../node_modules/bootstrap/scss/mixins";

    $theme-colors: map-remove($theme-colors, "info", "light", "dark");

    // Optional
    @import "../node_modules/bootstrap/scss/root";
    @import "../node_modules/bootstrap/scss/reboot";
    @import "../node_modules/bootstrap/scss/type";
    ...
    ```

  * 函数

    \`\`\`scss @function color\($key: "blue"\) { @return map-get\($colors, $key\); }

  @function theme-color\($key: "primary"\) { @return map-get\($theme-colors, $key\); }

  @function gray\($key: "100"\) { @return map-get\($grays, $key\); }

  ```text
  这样可以让你从一个Sass映射里挑颜色
  ```scss
  .custom-element {
    color: gray("100");
    background-color: theme-color("dark");
  }
  ```

## 布局

### 容器Container

The most basic layout element in Bootstrap and are required when using **default grid system.**

Bootstrap comes with three different containers:

* .container, which sets a max-width at each responsive breakpoint
* .container-fluid, which is width: 100% at all breakpoints
* .container-{breakpoint}, which is width: 100% until the specified breakpoint

### Z-index

```css
$zindex-dropdown:          1000 !default;
$zindex-sticky:            1020 !default;
$zindex-fixed:             1030 !default;
$zindex-modal-backdrop:    1040 !default;
$zindex-modal:             1050 !default;
$zindex-popover:           1060 !default;
$zindex-tooltip:           1070 !default;
```

## 例子

### Alert

```markup
<div class="alert alert-primary" role="alert">
  A simple primary alert—check it out!
</div>
<div class="alert alert-secondary" role="alert">
  A simple secondary alert—check it out!
</div>
<div class="alert alert-success" role="alert">
  A simple success alert—check it out!
</div>
<div class="alert alert-danger" role="alert">
  A simple danger alert—check it out!
</div>
<div class="alert alert-warning" role="alert">
  A simple warning alert—check it out!
</div>
<div class="alert alert-info" role="alert">
  A simple info alert—check it out!
</div>
<div class="alert alert-light" role="alert">
  A simple light alert—check it out!
</div>
<div class="alert alert-dark" role="alert">
  A simple dark alert—check it out!
</div>
```

#### Link color

Use the .alert-link utility class to quickly provide matching colored links within any alert.

```markup
<div class="alert alert-primary" role="alert">
  A simple primary alert with <a href="#" class="alert-link">an example link</a>. Give it a click if you like.
</div>
<div class="alert alert-secondary" role="alert">
  A simple secondary alert with <a href="#" class="alert-link">an example link</a>. Give it a click if you like.
</div>
<div class="alert alert-success" role="alert">
  A simple success alert with <a href="#" class="alert-link">an example link</a>. Give it a click if you like.
</div>
<div class="alert alert-danger" role="alert">
  A simple danger alert with <a href="#" class="alert-link">an example link</a>. Give it a click if you like.
</div>
<div class="alert alert-warning" role="alert">
  A simple warning alert with <a href="#" class="alert-link">an example link</a>. Give it a click if you like.
</div>
<div class="alert alert-info" role="alert">
  A simple info alert with <a href="#" class="alert-link">an example link</a>. Give it a click if you like.
</div>
<div class="alert alert-light" role="alert">
  A simple light alert with <a href="#" class="alert-link">an example link</a>. Give it a click if you like.
</div>
<div class="alert alert-dark" role="alert">
  A simple dark alert with <a href="#" class="alert-link">an example link</a>. Give it a click if you like.
</div>
```

#### headings, paragraphs and dividers

```markup
<div class="alert alert-success" role="alert">
  <h4 class="alert-heading">Well done!</h4>
  <p>Aww yeah, you successfully read this important alert message. This example text is going to run a bit longer so that you can see how spacing within an alert works with this kind of content.</p>
  <hr>
  <p class="mb-0">Whenever you need to, be sure to use margin utilities to keep things nice and tidy.</p>
</div>
```

### 边框

#### 增加

```markup
<span class="border"></span>
<span class="border-top"></span>
<span class="border-right"></span>
<span class="border-bottom"></span>
<span class="border-left"></span>
```

#### 减少

```markup
<span class="border-0"></span>
<span class="border-top-0"></span>
<span class="border-right-0"></span>
<span class="border-bottom-0"></span>
<span class="border-left-0"></span>
```

#### 颜色

```markup
<span class="border border-primary"></span>
<span class="border border-secondary"></span>
<span class="border border-success"></span>
<span class="border border-danger"></span>
<span class="border border-warning"></span>
<span class="border border-info"></span>
<span class="border border-light"></span>
<span class="border border-dark"></span>
<span class="border border-white"></span>
```

#### 边框半径

```markup
<img src="..." alt="..." class="rounded">
<img src="..." alt="..." class="rounded-top">
<img src="..." alt="..." class="rounded-right">
<img src="..." alt="..." class="rounded-bottom">
<img src="..." alt="..." class="rounded-left">
<img src="..." alt="..." class="rounded-circle">
<img src="..." alt="..." class="rounded-pill">
<img src="..." alt="..." class="rounded-0">
```

#### 大小

```markup
<img src="..." alt="..." class="rounded-sm">
<img src="..." alt="..." class="rounded-lg">
```

### 按钮Button

```markup
<button type="button" class="btn btn-primary">Primary</button>
<button type="button" class="btn btn-secondary">Secondary</button>
<button type="button" class="btn btn-success">Success</button>
<button type="button" class="btn btn-danger">Danger</button>
<button type="button" class="btn btn-warning">Warning</button>
<button type="button" class="btn btn-info">Info</button>
<button type="button" class="btn btn-light">Light</button>
<button type="button" class="btn btn-dark">Dark</button>

<button type="button" class="btn btn-link">Link</button>
```

### 卡片

[https://getbootstrap.com/docs/4.5/components/card/](https://getbootstrap.com/docs/4.5/components/card/)

重点看一下卡片和图片的结合，还有image overlay。

### Carousel

[https://getbootstrap.com/docs/4.5/components/carousel/](https://getbootstrap.com/docs/4.5/components/carousel/)

可以加不同的indicators，底部caption，渐变效果

### Collapse

[https://getbootstrap.com/docs/4.5/components/collapse/](https://getbootstrap.com/docs/4.5/components/collapse/)

* .collapse hides content
* .collapsing is applied during transitions
* .collapse.show shows content

可以用a tag也可以用button来触发

```markup
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#collapseExample" role="button" aria-expanded="false" aria-controls="collapseExample">
    Link with href
  </a>
  <button class="btn btn-primary" type="button" data-toggle="collapse" data-target="#collapseExample" aria-expanded="false" aria-controls="collapseExample">
    Button with data-target
  </button>
</p>
<div class="collapse" id="collapseExample">
  <div class="card card-body">
    Anim pariatur cliche reprehenderit, enim eiusmod high life accusamus terry richardson ad squid. Nihil anim keffiyeh helvetica, craft beer labore wes anderson cred nesciunt sapiente ea proident.
  </div>
</div>
```

支持多个触发

* 只需要把JQuery selector传进data-target即可

```text
A <button> or <a> can show and hide multiple elements by referencing them with a JQuery selector in its href or data-target attribute. Multiple <button> or <a> can show and hide an element if they each reference it with their href or data-target attribute
```

支持Accordion，只需要把`.wrapper`加上就行

### Dropdown

[https://getbootstrap.com/docs/4.5/components/dropdowns/](https://getbootstrap.com/docs/4.5/components/dropdowns/)

支持a和button触发

```markup
<div class="dropdown">
  <button class="btn btn-secondary dropdown-toggle" type="button" id="dropdownMenuButton" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
    Dropdown button
  </button>
  <div class="dropdown-menu" aria-labelledby="dropdownMenuButton">
    <a class="dropdown-item" href="#">Action</a>
    <a class="dropdown-item" href="#">Another action</a>
    <a class="dropdown-item" href="#">Something else here</a>
  </div>
</div>
```

创建split button

```markup
<button type="button" class="btn btn-danger">Action</button>
<button type="button" class="btn btn-danger dropdown-toggle dropdown-toggle-split" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
  <span class="sr-only">Toggle Dropdown</span>
</button>
```

支持不同方向，不如dropup和dropright/left，加入对应的类在大的Wrapper即可

`Add .active to items in the dropdown to style them as active.`

`Add .disabled to items in the dropdown to style them as disabled.`

菜单的对齐：默认是100% from top左对齐

`Add .dropdown-menu-right to a .dropdown-menu to right align the dropdown menu`

`Use data-offset or data-reference to change the location of the dropdown.`

```markup
<div class="d-flex">
  <div class="dropdown mr-1">
    <button type="button" class="btn btn-secondary dropdown-toggle" id="dropdownMenuOffset" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false" data-offset="10,20">
      Offset
    </button>
    <div class="dropdown-menu" aria-labelledby="dropdownMenuOffset">
      <a class="dropdown-item" href="#">Action</a>
      <a class="dropdown-item" href="#">Another action</a>
      <a class="dropdown-item" href="#">Something else here</a>
    </div>
  </div>
  <div class="btn-group">
    <button type="button" class="btn btn-secondary">Reference</button>
    <button type="button" class="btn btn-secondary dropdown-toggle dropdown-toggle-split" id="dropdownMenuReference" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false" data-reference="parent">
      <span class="sr-only">Toggle Dropdown</span>
    </button>
    <div class="dropdown-menu" aria-labelledby="dropdownMenuReference">
      <a class="dropdown-item" href="#">Action</a>
      <a class="dropdown-item" href="#">Another action</a>
      <a class="dropdown-item" href="#">Something else here</a>
      <div class="dropdown-divider"></div>
      <a class="dropdown-item" href="#">Separated link</a>
    </div>
  </div>
</div>
```

### 表单Form

[https://getbootstrap.com/docs/4.5/components/forms/](https://getbootstrap.com/docs/4.5/components/forms/)

[https://getbootstrap.com/docs/4.5/components/input-group/](https://getbootstrap.com/docs/4.5/components/input-group/)

Be sure to use an appropriate type attribute on all inputs \(e.g., email for email address or number for numerical information\) to take advantage of newer input controls like email verification, number selection, and more.

```markup
<form>
  <div class="form-group">
    <label for="exampleInputEmail1">Email address</label>
    <input type="email" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp">
    <small id="emailHelp" class="form-text text-muted">We'll never share your email with anyone else.</small>
  </div>
  <div class="form-group">
    <label for="exampleInputPassword1">Password</label>
    <input type="password" class="form-control" id="exampleInputPassword1">
  </div>
  <div class="form-group form-check">
    <input type="checkbox" class="form-check-input" id="exampleCheck1">
    <label class="form-check-label" for="exampleCheck1">Check me out</label>
  </div>
  <button type="submit" class="btn btn-primary">Submit</button>
</form>
```

### Modal

[https://getbootstrap.com/docs/4.5/components/modal/](https://getbootstrap.com/docs/4.5/components/modal/)

```markup
<!-- Button trigger modal -->
<button type="button" class="btn btn-primary" data-toggle="modal" data-target="#exampleModal">
  Launch demo modal
</button>

<!-- Modal -->
<div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <div class="modal-body">
        ...
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Save changes</button>
      </div>
    </div>
  </div>
</div>
```

### Pagination

[https://getbootstrap.com/docs/4.5/components/pagination/](https://getbootstrap.com/docs/4.5/components/pagination/)

```markup
<nav aria-label="Page navigation example">
  <ul class="pagination">
    <li class="page-item"><a class="page-link" href="#">Previous</a></li>
    <li class="page-item"><a class="page-link" href="#">1</a></li>
    <li class="page-item"><a class="page-link" href="#">2</a></li>
    <li class="page-item"><a class="page-link" href="#">3</a></li>
    <li class="page-item"><a class="page-link" href="#">Next</a></li>
  </ul>
</nav>
```

可以加icon，记得要 provide proper screen reader support with aria attributes.

`. Use .disabled for links that appear un-clickable and .active to indicate the current page.`

### Progress

[https://getbootstrap.com/docs/4.5/components/progress/](https://getbootstrap.com/docs/4.5/components/progress/)

可以加标签，高度，背景，多个进度，线条填充

可以动画：

```markup
<div class="progress">
  <div class="progress-bar progress-bar-striped progress-bar-animated" role="progressbar" aria-valuenow="75" aria-valuemin="0" aria-valuemax="100" style="width: 75%"></div>
</div>
```

### Scrollspy

[https://getbootstrap.com/docs/4.5/components/scrollspy/\#item-3-1](https://getbootstrap.com/docs/4.5/components/scrollspy/#item-3-1)

滚动的时候，加进度tag显示

### Spinner

[https://getbootstrap.com/docs/4.5/components/spinners/](https://getbootstrap.com/docs/4.5/components/spinners/)

Border spinner，加颜色和间距，对齐摆放方式，大小，和按钮

### Tooltips

[https://getbootstrap.com/docs/4.5/components/tooltips/](https://getbootstrap.com/docs/4.5/components/tooltips/)

```markup
<button type="button" class="btn btn-secondary" data-toggle="tooltip" data-placement="top" title="Tooltip on top">
  Tooltip on top
</button>
<button type="button" class="btn btn-secondary" data-toggle="tooltip" data-placement="right" title="Tooltip on right">
  Tooltip on right
</button>
<button type="button" class="btn btn-secondary" data-toggle="tooltip" data-placement="bottom" title="Tooltip on bottom">
  Tooltip on bottom
</button>
<button type="button" class="btn btn-secondary" data-toggle="tooltip" data-placement="left" title="Tooltip on left">
  Tooltip on left
</button>
```

### Grid

`$enable-grid-classes`: Enables the generation of CSS classes for the grid system \(e.g., `.container, .row, .col-md-1`, etc.\). 默认是True

`Add .pagination-lg or .pagination-sm for additional sizes.`

## Spacing

Bootstrap has a wide range of responsive margin and padding utility classes. They work for all breakpoints:

**xs** \(&lt;=576px\), **sm** \(&gt;=576px\), **md** \(&gt;=768px\), **lg** \(&gt;=992px\) or **xl** \(&gt;=1200px\)\)

The classes are used in the format:

**{property}{sides}-{size}** for xs & **{property}{sides}-{breakpoint}-{size}** for sm, md, lg, and xl.

**m** - sets margin

**p** - sets padding

**t** - sets margin-top or padding-top

**b** - sets margin-bottom or padding-bottom

**l** - sets margin-left or padding-left

**r** - sets margin-right or padding-right

**x** - sets both padding-left and padding-right or margin-left and margin-right

**y** - sets both padding-top and padding-bottom or margin-top and margin-bottom

**blank** - sets a margin or padding on all 4 sides of the element

**0** - sets **margin** or **padding** to 0

**1** - sets **margin** or **padding** to .25rem \(4px if font-size is 16px\)

**2** - sets **margin** or **padding** to .5rem \(8px if font-size is 16px\)

**3** - sets **margin** or **padding** to 1rem \(16px if font-size is 16px\)

**4** - sets **margin** or **padding** to 1.5rem \(24px if font-size is 16px\)

**5** - sets **margin** or **padding** to 3rem \(48px if font-size is 16px\)

**auto** - sets margin to auto

See more at [Bootstrap 4.5 - Spacing](https://getbootstrap.com/docs/4.5/utilities/spacing/)

[Read more in w3schools](https://www.w3schools.com/bootstrap4/bootstrap_utilities.asp)

[https://stackoverflow.com/questions/24175998/meaning-of-numbers-in-col-md-4-col-xs-1-col-lg-2-in-bootstrap](https://stackoverflow.com/questions/24175998/meaning-of-numbers-in-col-md-4-col-xs-1-col-lg-2-in-bootstrap)

