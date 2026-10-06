# async/await

## Definition

Async/await are part of javascript syntax. They utilize Promise objects to write asyncronous code that reads like syncronous code.

async keyword: when placed before a function definition, it ensures that the function should return a promise.
await keyword: should be placed inside of an async before a promise function call. It stops the excution of the code, temperarily, until a promise is either fulfilled or rejected.


## Examples

### Promise chain vs async/await

```js
async function loadUser() {
  try {
    const res = await fetch('/api/user');
    const user = await res.json();
    console.log(user);
  } catch (err) {
    console.error(err);
  }
}
```


```js
// Sequential excution
const users = await fetch('/api/users').then((r) => r.json());
const posts = await fetch('/api/posts').then((r) => r.json());

// Parallel
const [users2, posts2] = await Promise.all([
  fetch('/api/users').then((r) => r.json()),
  fetch('/api/posts').then((r) => r.json()),
]);
```