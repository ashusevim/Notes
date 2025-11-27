# Closure

- if a variable declared inside a code block using 'let', it's only visible inside that block.

```jsx
{
	let co = 5;

	alert(co); //5
}

alert(co); // co is not defined......

```