# SCSS

## Map and foreach

```css
@each $module in $modules {
  &.#{nth($module, 1)} {
    $map: nth($module, 2);
    aside[role="complementary"] {
      margin-top: calc(
        -30px + #{$vmenu_height} * #{map-get($map, "offset")}
      );
      > div {
        &.sticky {
          position: fixed;
          top: 0;

          &.sticky-admin {
            top: 104px;
          }
        }

        background-color: map-get($map, "overlay");
        min-width: 200px;
        // padding-top: 30px;

        li {
          padding: 24px 24px 12px;
          display: flex;
          &.active {
            > a {
              color: map-get($map, "active");
              border-bottom: 1px solid map-get($map, "active");
            }
          }
          > a {
            color: white;
            padding-bottom: 12px;
            width: 100%;
            border-bottom: 1px solid white;

            &:hover {
              opacity: 0.7;
              text-decoration: none;
            }
          }
        }
      }
    }
    .main_wrapper .main_right {
      background-image: map-get($map, "background-image");
    }
  }
}

$vmenu_height: 68px;

$aup: (
  background-image: url("../images/03-aup-background.jpg"),
  offset: 0,
  overlay: rgba(8, 79, 131, 0.42),
  active: darken(rgb(8, 79, 131), 15%),
);

$vpc: (
  background-image: url("../images/05-vpc-background.jpg"),
  offset: 0,
  overlay: rgba(61, 161, 83, 0.51),
  active: darken(rgb(61, 161, 83), 15%),
);

$permissionslip: (
  background-image: url("../images/04-permission-slip-background.jpg"),
  offset: 0,
  overlay: rgba(229, 81, 81, 0.56),
  active: darken(rgb(229, 81, 81), 15%),
);

$mydocs: (
  background-image: url("../images/06-my-docs-background.jpg"),
  offset: 0,
  overlay: rgba(240, 132, 40, 0.7),
  active: darken(rgb(240, 132, 40), 15%),
);

$modules: aup $aup, vpc $vpc, permissionslip $permissionslip, mydocs $mydocs;

```

## Media query

{% embed url="https://stackoverflow.com/questions/36957904/media-queries-in-sass" %}

This is what I use for a Mixin with sass, it allows me to quickly reference the breakpoint that I want. obviously you can adjust the media query list to suite your project mobile fist etc.

But it will jin multiple queries for you as I believe you're asking for.

```text
$size__site_content_width: 1024px;

/* Media Queries */ Not necessarily correct, edit these at will 
$media_queries : (
    'mobile'      : "only screen and (max-width: 667px)",
    'tablet'      : "only screen and (min-width: 668px) and (max-width: $size__site_content_width)",
    'desktop'     : "only screen and (min-width: ($size__site_content_width + 1))",
    'retina2'     : "only screen and (-webkit-min-device-pixel-ratio: 2) and (min-resolution: 192dpi)",
    'retina3'     : "only screen and (-webkit-min-device-pixel-ratio: 3) and (min-resolution: 288dpi)",
    'landscape' : "screen and (orientation:landscape) ",    
    'portrait'     : "screen and (orientation:portrait) "
);

@mixin for_breakpoint($breakpoints) {
    $conditions : ();
    @each $breakpoint in $breakpoints {
        // If the key exists in the map
        $conditions: append(
            $conditions,
            #{inspect(map-get($media_queries, $breakpoint))},
            comma
        );
    }

    @media #{$conditions} {
        @content;
    }

}
```

Use it like this in your scss:

```text
#masthead {
    background: white;
    border-bottom:1px solid #eee;
    height: 90px;
    padding: 0 20px;
    @include for_breakpoint(mobile desktop) {
        height:70px;
        position:fixed;
        width:100%;
        top:0;
    }
}
```

Then this will compile to:

```text
#masthead { 
  background: white;
  border-bottom: 1px solid #eee;
  height: 90px;
  padding: 0 20px;
}

@media only screen and (max-width: 667px), only screen and (min-width: 1025px) {
  #masthead {
    height: 70px;
    position: fixed;
    width: 100%;
    top: 0;
  }
}
```

