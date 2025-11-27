# Flexbox

### Flexbox makes it easier to design flexible responsive layout structure without using float or positioning.

- The flex container becomes flexible by setting the `display` property to `flex`:

```css
.flex-container {
		display: flex;
}

```

- The flex-wrap property: it specifies whether the flex items should wrap or not.

```css
.flex-container{
		flex-wrap: wrap; /* flex items will wrap */
		flex-wrap: nowrap; /* flex items will not wrap */
}

```

- align-content property: property is used to align the flex lines.
- align-items used when we have a single line.
- align-content used when we have multiple lines.
- `flex-flow: <flex-direction> <flex-wrap>`
- **Gap**: this property set the gap between rows and columns.
- row-gap: it specifies the gap between the rows.
- column-gap: it specifies the gap between the columns.