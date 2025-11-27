# DOM

1. `getElementsByTagName` will return a <span style="color:green">HTMLCollection</span>. and there is only one button, but `getElementsByTagName` will still return collection, so ==you need to select the first element from that collection==.
2. How to create a new element and add to empty div tag

```jsx
// this is our empty div element
let content = document.querySelector(".content")

//for example we want to create <img> tag
let IMG = document.createElement("img")

//add image url to its source
IMG.src = `${imageURL}&`

//append the child of div and insert created new <img> tag
content.appendChild(IMG)

```

### Using **'classList.add'**

1. Adds one or more class names to an element without affecting the existing class names.
    - Advantages:
        - does not overwrite existing class names.
        - can be used to toggle or remove specific classes.

### Using 'className'

1. sets or gets the value of the class attribute.
    - Advantages:
        - useful when you want to overwrite all existing classes.

### The `preventDefault()` method is used to stop the default action of an element from happening. It is commonly used in form submission