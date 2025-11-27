# Grid

- `display: Grid` Grid items are placed in rows.
- `minmax()` it accepts 2 arguments: minimum size of the track and maximum size.
- `grid-gaps` **grid-colums-gap** and **grid-rows-gap** properties create gutters between colums and rows.
    - ex `grid-gap: 100px auto` 100px is grid-rows-gap and auto is grid-colums-gap.
    - ex `grid-gap: 100px` one value sets equal row and column gaps.
- CSS Grid layout module offers a grid-based layout system, making it easier to design web pages without using floats and positioning.
- element becomes a grid container when:

```css
.grid-container{
		display: grid;
}

```

- <span style="color: lightblue">Vertical lines</span> of grid-items are called **columns**.
- <span style="color:lightblue">Horizontal lines</span> of grid item are called rows.

### Grid Gaps

The spaces between each column/row are called gaps.

```
gap: <row-gap> <column-gap>
```

![](https://i.imgur.com/6JlPik8.png)

### grid-template-columns

<span style="color:cyan">It defines the number of colums in your grid layout.</span>

- It can define the width of every column.
- if yoor grid layout contains 4 columns,
`grid-template-columns: auto auto auto auto`
if all colums should have the same width.
- It can also used to specify the size(width) of the colums.

### grid-template-rows

<span style="color:cyan">This property defines the height of each row.</span>