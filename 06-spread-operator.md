# Spread Operator (`...`)

## Definition

it's placed before a list or iterable. It unpacks values from an array, for example
```js
arr2 = [...ar1]
``` 
creates a new array `arr2` with the same elements as `arr1`.

it is important to distinuish it from the reset operator, which gathers multiple elements into a sinlge array:
for example:
```js
function sum(...numbers) {
  return numbers.reduce((a, b) => a + b, 0);
}
```