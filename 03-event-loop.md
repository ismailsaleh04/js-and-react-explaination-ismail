# Event Loop

## Definition

Because js is a **single-threaded** languae, it is slow. **event loop** solves this. It works with timers, network, etc...

The main parts of the event loop are:

1. Call stack: holds the funcion currently in excution.
2. apis: where async work is currently happening.
3. microtasks queues: functions that return promises.
4. macrotasks (tasks): functions that 

The loop repeats:

1. Run everything on the call stack until it's empty.
2. Run **all** microtasks (including ones added while running them).
3. Take **one** macrotask, run it, then go back to step 2.

**Key rule:** microtasks (promises) always run before the next macrotask (timers).

## Examples

### Order of execution

```js
console.log('1 - sync');

setTimeout(() => console.log('4 - macrotask (setTimeout)'), 0);

Promise.resolve().then(() => console.log('3 - microtask (promise)'));

console.log('2 - sync');

// Output:
// 1 - sync
// 2 - sync
// 3 - microtask (promise)
// 4 - macrotask (setTimeout)
```

### async/await and the event loop

```js
async function run() {
  console.log('A');
  await null;          // everything after this becomes a microtask
  console.log('C');
}

run();
console.log('B');

// A, B, C
```

### Blocking the loop

```js
setTimeout(() => console.log('timer'), 0);

const start = Date.now();
while (Date.now() - start < 3000) {} // heavy sync work blocks for 3s

console.log('done');
// "done" after 3s, then "timer" — the timer could not run while the stack was busy
```

## Use cases / why it matters

- **Predicting output order** of mixed sync, promise and timer code (common interview question).
- **Avoiding frozen UIs:** in React Native, long synchronous work on the JS thread blocks touches, animations and rendering. Split heavy work, move it off the JS thread, or use `InteractionManager.runAfterInteractions`.
- Understanding why `setTimeout(fn, 0)` doesn't run "immediately".
- Understanding why state updates and effects run after the current code finishes.
