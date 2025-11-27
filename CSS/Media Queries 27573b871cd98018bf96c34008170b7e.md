# Media Queries

Media queries are used when we want to customize our website's presentation according to the **user's screen size**. With the help of media queries, user can display **different markups** based upon the **device's general type(mobile, desktop, tablet)**.

It is a **logical operation**. Whenever a **media query** **becomes** **true**, then the **related CSS is applied to the target element**.

- **portrait: height > width**
- **landscape: width > height**

```css
@media not|only mediatype and (expressions){
	/* css code  */
}

/* example 1 */
@media only screen and (max-width: 450px){
	body{
		background-color: red;
	}
}

/* example 2 */
@media only screen and (orientation: landscape){
	body{
		background-color: cyan;
		flex-direction: column;
	}
}

```

- `@media` signifies the start of a media query.