# Promises

## Definition

A javascript object that represents a future outcome (a value we don't have yet).
it has three states: 
* pending
* fulfilled
* rejected

Many functions in javascript return a Promise type, such as `fetch` or `timeout`.
A custom Promise can be created though through the Promise constructor, by passing a function that takes two parameters: resolve and reject, each one is a function that does something when the promise either is fulfilled or is rejected.


## Example

creating custom 

```ts
function promiseCreator(ismail: string): Promise {
  return new Promise((resolve, reject) => {
    if (ismail === "ismail") setTimeout(() => resolve("ismail is created"));
    reject(new Error("ismail failed to create"));
  });
}

const ismailPromise1 = promiseCreator("ismail");
const ismailPromise2 = promiseCreator("not-ismail");

ismailPromise1.then(result => console.log(`result:${result}`));
ismailPromise2.then(result => console.log(`result:${result}`));
```