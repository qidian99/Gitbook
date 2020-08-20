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



