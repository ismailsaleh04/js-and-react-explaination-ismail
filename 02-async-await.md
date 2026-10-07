# async/await

## Definition

Async/await are part of javascript syntax. They utilize Promise objects to write asyncronous code that reads like syncronous code.

async keyword: when placed before a function definition, it ensures that the function should return a promise.
await keyword: should be placed inside of an async before a promise function call. It stops the excution of the code, temperarily, until a promise is either fulfilled or rejected.

it is preferrerd to place the async/await logic inside a try/catch block, so that whena promise fails, we catch it with `catch`.