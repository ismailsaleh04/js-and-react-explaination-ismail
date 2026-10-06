# map / filter / reduce

## Definition

1. map: a method that transforms elements of an array or an iterable:
example:
```js
doubledarray = array.map((i) => i*2);
```

2. filter: a method that filters the elements of an array by a condition.
example:
```js
evenarray = array.filter((i) => i%2 == 0);
```

3. reduce: a method that returns a single output value, by accumulating the elements of the array in a specified manner.
exampel:
```js
summation = array.reduce((i, s = 0) => s+i, 0);
```