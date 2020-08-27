# Material UI

## withStyles and Theme

{% embed url="https://codex.happyfuncorp.com/styling-and-theming-with-material-ui-react-material-design-3ba2d2f0ef25" %}

## 3 ways of UI styling

{% embed url="https://blog.bitsrc.io/a-better-way-to-style-material-ui-80c7707ad525" %}

## Customize styles

{% embed url="https://v1.material-ui.com/customization/overrides/" %}

## makeStyles explained

{% embed url="https://stackoverflow.com/questions/57220059/internal-implementation-of-makestyles-in-react-material-ui" %}

## CSS to React styles

{% embed url="https://staxmanade.com/CssToReact/" %}

## Themes

{% embed url="https://v1.material-ui.com/customization/themes/\#typography" %}

```text
import { createMuiTheme } from '@material-ui/core/styles';

const fontSize = 16;

export const theme = createMuiTheme({
  typography: {
    fontSize,
    display4: {
      fontSize,
    },
    display3: {
      fontSize,
    },
    display2: {
      fontSize,
    },
    display1: {
      fontSize,
    },
    headline: {
      fontSize,
    },
    title: {
      fontSize,
    },
    subheading: {
      fontSize,
    },
    body2: {
      fontSize,
    },
    body1: {
      fontSize,
    },
    caption: {
      fontSize,
    },
    button: {
      fontSize,
    },
  },
  overrides: {
    MuiTableCell: {
      body: {
        fontSize: 16
      },
    },
  },
  palette: {
    primary: {
      main: "#CB7932",
    },
    secondary: {
      main: "#dba170",
    },
  },
  status: {
    danger: 'orange',
  },
});


```

## Spacing

As per the docs

## Ripple animation

{% embed url="https://medium.com/@dhilipkmr/ripple-in-react-3162875cc9af" %}



* xs, extra-small: 0px or larger
* sm, small: 600px or larger
* md, medium: 960px or larger
* lg, large: 1280px or larger
* xl, extra-large: 1920px or larger

Reference [https://material-ui.com/layout/breakpoints/](https://material-ui.com/layout/breakpoints/)

> For example what would be the reason for providing xs={true} or md={false} be? And how might I have learned the reason on my own? \(is there some fundamental knowledge I'm lacking?\)

This `xs={true}` means that the column will take up an equal space that is in a given row

## Toolbar

{% embed url="https://github.com/mui-org/material-ui/issues/10076\#issuecomment-361232810" %}

## Tooltip

on component

{% embed url="https://material-ui.com/components/tooltips/" %}



