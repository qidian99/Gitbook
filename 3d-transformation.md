# 3D Transformation

## RotateX

{% embed url="https://drafts.csswg.org/css-transforms-2/\#funcdef-rotatex" %}

{% embed url="https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function/rotateX" %}

The **`rotateX()`** [CSS](https://developer.mozilla.org/en-US/docs/Web/CSS) function defines a transformation that rotates an element around the abscissa \(horizontal axis\) without deforming it. Its result is a [`<transform-function>`](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function) data type.



### Syntax <a id="Syntax"></a>

The amount of rotation created by `rotateX()` is specified by an [`<angle>`](https://developer.mozilla.org/en-US/docs/Web/CSS/angle). If positive, the movement will be clockwise; if negative, it will be counter-clockwise.

```text
rotateX(a)
```

#### Values <a id="Values"></a>

`a`Is an [`<angle>`](https://developer.mozilla.org/en-US/docs/Web/CSS/angle) representing the angle of the rotation. A positive angle denotes a clockwise rotation, a negative angle a counter-clockwise one.

保证X坐标不变，注意他使用Row Vector来乘的transformation matrix。用Unit vector来决定sine和cosine的值。

![](.gitbook/assets/image%20%2860%29.png)

## RotateY

The **`rotateY()`** [CSS](https://developer.mozilla.org/en-US/docs/Web/CSS) function defines a transformation that rotates an element around the ordinate \(vertical axis\) without deforming it. Its result is a [`<transform-function>`](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function) data type.

![](.gitbook/assets/image%20%2861%29.png)





