# CSS

## Height take remaining space

{% embed url="https://stackoverflow.com/questions/90178/make-a-div-fill-the-height-of-the-remaining-screen-space" %}

## Grid Order



Grid Layout provides multiple methods for re-arranging grid items. I've listed four below.

1. The **`grid-template-areas`** property
2. Line-based placement
3. The **`order`** property
4. The **`dense`** function of the `grid-auto-flow` property. _\(Possibly the simplest, easiest and most robust solution for this type of layout, as it works for any number of grid items.\)_

Here's the original layout:

```text
grid-container {
  display: grid;
  grid-template-columns: 15% 1fr 25%;
  grid-auto-rows: 50px; /* for demo */
  grid-gap: 10px;
}

/* non-essential decorative styles */
grid-item {
  border: 1px solid gray;
  background-color: lightgreen;
  display: flex;
  align-items: center;
  justify-content: center;
}
grid-item:nth-child(2) {
  background-color: orange;
}
```

```text
<grid-container>
  <grid-item>1</grid-item>
  <grid-item>2</grid-item>
  <grid-item>3</grid-item>
</grid-container>
```

 Run code snippetExpand snippet

[**jsFiddle demo 1**](https://jsfiddle.net/3mke2k69/5/)

### 1. The `grid-template-areas` property

The `grid-template-areas` property allows you to arrange your layout using ASCII visual art.

Place the grid area names \(which are defined for each element\) in the position where you want them to appear.

Show code snippet

[**jsFiddle demo 2**](https://jsfiddle.net/3mke2k69/6/)

_From the specification:_

> [**7.3. Named Areas: the `grid-template-areas` property**](https://www.w3.org/TR/css3-grid-layout/#grid-template-areas-property)
>
> This property specifies _**named grid areas**_, which are not associated with any particular grid item, but can be referenced from the grid-placement properties.
>
> The syntax of the `grid-template-areas` property also provides a visualization of the structure of the grid, making the overall layout of the grid container easier to understand.
>
> All strings must have the same number of columns, or else the declaration is invalid.
>
> If a named grid area spans multiple grid cells, but those cells do not form a single filled-in rectangle, the declaration is invalid.
>
> _Note: Non-rectangular or disconnected regions may be permitted in a future version of this module._

### 2. Line-based Placement

Use `grid-row-start`, `grid-row-end`, `grid-column-start`, `grid-column-end`, or their shorthands, `grid-row` and `grid-column`, to set a grid item's size and location in the grid.

Show code snippet

[**jsFiddle demo 3**](https://jsfiddle.net/3mke2k69/7/)

_From the specification:_

> [**8.3. Line-based Placement: the `grid-row-start`, `grid-column-start`, `grid-row-end`, and `grid-column-end` properties**](https://www.w3.org/TR/css3-grid-layout/#line-placement)

### 3. The `order` property

The `order` property in Grid Layout does the same as in Flex Layout.

Show code snippet

[**jsFiddle demo 3**](https://jsfiddle.net/3mke2k69/8/)

_From the specification:_

> [**6.3. Reordered Grid Items: the `order` property**](https://www.w3.org/TR/css3-grid-layout/#order-property)

### 4. The `dense` function of the `grid-auto-flow` property

This solution is possibly the simplest, easiest and most robust of all presented here, as it works for any number of grid items.

Although this solution doesn't provide any major advantage over the previous three _in the layout described in the question_, which consists of only three grid items, it offers huge benefits when the number of grid items is higher or dynamically-generated.

This question and the solutions above address this:

```text
01  03
02  02
```

But let's say we need this:

```text
01  03
02  02

04  06
05  05

07  09
08  08

10  12
11  11

and so on...
```

Using `grid-template-areas`, line-based placement and `order`, the solution would require a lot of hard-coding \(if we knew the number of total items\) and maybe CSS custom properties and/or JavaScript \(if the items were generated dynamically\).

The **`dense`** function of CSS Grid, however, can handle both scenarios simply and easily.

By applying `grid-auto-flow: dense` to the container, each third item will backfill the empty cell created as a result of consecutive ordering.

```text
grid-container {
  display: grid;
  grid-template-columns: 15% 1fr 25%;
  grid-auto-rows: 50px;
  grid-gap: 10px;
}

@media (max-width: 500px) {
  grid-container {
    grid-template-columns: 1fr 1fr;
    grid-auto-flow: dense;     /* KEY RULE; deactive to see what happens without it;
                                  defaults to "sparse" (consecutive ordering) value */
  }
  grid-item:nth-child(3n + 2) {
    grid-column: 1 / 3;
  }
}

/* non-essential decorative styles */
grid-item {
  border: 1px solid gray;
  background-color: lightgreen;
  display: flex;
  align-items: center;
  justify-content: center;
}
grid-item:nth-child(-n + 3)                    { background-color: aqua; }
grid-item:nth-child(n + 4):nth-child(-n + 6)   { background-color: lightgreen; }
grid-item:nth-child(n + 7):nth-child(-n + 9)   { background-color: orange; }
grid-item:nth-child(n + 10):nth-child(-n + 12) { background-color: lightgray; }
```

```text
<grid-container>
  <grid-item>1</grid-item>
  <grid-item>2</grid-item>
  <grid-item>3</grid-item>
  <grid-item>4</grid-item>
  <grid-item>5</grid-item>
  <grid-item>6</grid-item>
  <grid-item>7</grid-item>
  <grid-item>8</grid-item>
  <grid-item>9</grid-item>
  <grid-item>10</grid-item>
  <grid-item>11</grid-item>
  <grid-item>12</grid-item>
</grid-container>
```

 Run code snippetHide resultsFull page

[**jsFiddle demo 4**](https://jsfiddle.net/29oa3jds/)

_From the specification:_

> [**§7.7. Automatic Placement: the `grid-auto-flow` property**](https://www.w3.org/TR/css3-grid-layout/#grid-auto-flow-property)
>
> _**dense**_
>
> If specified, the auto-placement algorithm uses a “dense” packing algorithm, which attempts to fill in holes earlier in the grid if smaller items come up later.
>
> If omitted, a “sparse” algorithm is used, where the placement algorithm only ever moves “forward” in the grid when placing items, never backtracking to fill holes.

