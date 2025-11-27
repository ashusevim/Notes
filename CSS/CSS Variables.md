# CSS Variables

- entities defined by CSS authors that **represent specific values** to be **reused** throughout a document.

```html
<div class="navbar">
<ul>
	<li>Home</li>
	<li>Projects</li>
	<li>Favorite resources</li>
	<li>About me</li>
</ul>
</div>

```

```css
:root{
	--color: green;
	--seccolor: red;
}

.narbar{
	background-color: var(--color)
	color: var(--seccolor)
}

```

### Globle variables vs Local variables

- Global variables can be accessed throught the entire document, while local variables can be used only inside the selector where it is declared.
- global variable can be declared using `:root{}`