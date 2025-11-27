# Float and clear

- Float property **allows** the **balance** **between text and the image** shown beside it.
- It specifies what elements can **float beside** the **cleared element** and **on which side**.
- Float property value :
    1. left: element floats to the left.
    2. right: element floats to the right.
    3. none: element doesn't float. It is default.
    4. inherits: inherits float value of its parents.

### There is a property named 'clear', which is often used with float.

- `clear` property specifies what should happen with the element that is next to the floating element.
- `clear` property can have these values:
    1. `none` - The element is not pushed below left or right floated elements. This is default
    2. `left` - The element is pushed below left floated elements
    3. `right` - The element is pushed below right floated elements
    4. `both` - The element is pushed below both left and right floated elements
    5. `inherit` - The element inherits the clear value from its parent