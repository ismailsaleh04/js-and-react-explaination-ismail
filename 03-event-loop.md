# Event Loop

## Definition

Because js is a **single-threaded** languae, it is slow. **event loop** solves this. It works with timers, network actions, etc...

The main parts of the event loop are:

1. Call stack: holds the funcion currently in excution.
2. apis: where async work is currently happening.
3. microtasks queues: functions that return promises.
4. macrotasks (tasks): such as timeouts, i/o

The loop repeats:

1. Run everything on the call stack until it's empty.
2. Run **all** microtasks (including ones added while running them).
3. Take **one** macrotask, run it, then go back to step 2.
