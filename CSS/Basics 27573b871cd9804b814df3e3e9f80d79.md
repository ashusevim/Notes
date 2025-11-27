# Basics

### 1. To center an image within an `img` tag

- **Method 1:** using `text-align`

```css
.image-container {
  text-align: center;
}

```

- **Method 2:** using `flex box`

```css
.flex-container {
  display: flex;
  justify-content: center; /* horizontly */
  align-items: center; /* vertically */
}

```

---

### 2. How to select multiple elements in CSS?(applying one styling to multiple HTML elements)

- HTML Code

```html
<div>to be selected</div>
<span>this is also going to be selected</span>

```

- CSS code

```css
div, span{
	color: red;
	background-color: yellow;
}

```

### we can give multiple classes to single element

Example. `<div class="c1 c2"></div>`

---

### User agent style-sheet

- a set of predefined CSS rules that are built into a web browser and automatically added to web pages.