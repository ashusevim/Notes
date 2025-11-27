# Specificity and Cascade

### **Cascade Algorithm:** The cascade is the algorithm for solving conflicts where multiple CSS rules apply to an HTML element.

### Cascade algorithm has 4 distinct stages:

1. **Position and order of appearance:** the order in which your CSS rules appear
2. **specificity:** determines which CSS selector has the strongest match\
    - The order of specificity is:
    `inline style > ID selector > class or attribute > element selector > universal selector`
3. origin: the order in which CSS appears and where it comes from
4. Importance: !important flag in CSS is used to give a particular style rule the highest level of importance, overriding any other competing styles

---

### *specificity calculation

- assign a value to each part of the selector
    - universal selector: 0
    - element selector & pseudo-element: 1
    - class selectors, attribute selectors and pseudo-classes: 10
    - ID selectors: 10
    - inline styles: 1000