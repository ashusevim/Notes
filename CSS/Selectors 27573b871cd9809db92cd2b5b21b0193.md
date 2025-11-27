# Selectors

### 1. child selectors

```html
<div class="red">
	<p>this is paragraph</p>
<div>
```

```css
div p{
	background-color: red;
}
```

- p element should be the direct child of div element

### 2. descendant selectors

```css
div p{
	color: yellow;
	background-color: brown;
}
```

- p should be present inside the div element somewhere

### 3. universal selector

```css
*{
	background-color: black;
	margin: 40px;
}
```

### 4. pseudo selector

- `<a href="www.google.com">click on me</html>`

```css
a:hover{
	background-color: black;
}
```

- background-color will be changed when hover on the link

### selector grouping is a technique to apply styles to multiple elements at once

```html
<h1>this is main heading</h1>
<h2>this is secondary heading</h2>
```

```css
h1, h2{
	background-color: black;
}
```

- nth-child counting starts from **1**
- nth-last-child: select nth child from last

## Element can be dynamically inserted through css using this syntax:

```css
.boxes::before{
	content: "to be inserted";
}
```

- We can use `::after` also

```css
.boxes::after{
	content: "this will be inserted after";
}
```

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>More on css selectors</title>
    <style>
        .boxes {
            border-radius: 15px;
            padding: 0.5px;
        }

        .boxes * {
            background-color: green;
            padding: 20px;
            border: 2px solid;
            border-radius: 15px;
            margin: 10px;
        }

        /* [data-color="primary"] {
            background-color: black;
        } */

        /* .box:nth-child(odd) {
            background-color: black;
        } */

        /* .boxes::before {
            content: ;
        }

        .boxes::after{
            content;
        } */

        input {
            border: 1.5px solid black;
            border-radius: 7px;
            margin: 0px 11px;
        }

        input::placeholder {
            padding: 0px 6px;
            color: blueviolet;
        }
    </style>
</head>

<body>
    <div class="boxes">
        <div data-color="primary" class="box">Box 1</div>
        <div class="box">Box 2</div>
        <div class="box">Box 3</div>
    </div>
    <input type="text" placeholder="please type me">
</body>

</html>
```