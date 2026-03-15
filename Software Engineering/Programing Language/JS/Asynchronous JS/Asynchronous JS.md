## Event Loop
![[Untitled 11.png|Untitled 11.png]]

**Event Loop**: Triggers when call stack is clear
1. Dequeue and run the oldest task from the macrotask queue.
2. Execute all microtasks:
    - While the microtask queue is not empty:
        - Dequeue and run the oldest microtask.
3. Render changes if any.
4. If the macrotask queue is empty, wait till a macrotask appears.
5. Go to step 1.

**Heap:** the memory allocation for variables
**Stack:** function stack
**Macrotask/Event Queue:** asynchronous functions
	DOM events, setTimeout, setTimeInterval, Promise, callbacks in I/O operations like fetch
**Microtask Queue**: created by promises and hence await as well
## Object Promise
```JavaScript
let promise = new Promise((resolve, reject) => {
});
```
`resolve` and `reject` are callbacks provided by JS.
`resolve(value)`, `reject(error)`
![[Untitled 1 4.png|Untitled 1 4.png]]
`state` and `result` are two internal properties (cannot be accessed from the outside) of promise.
`state` in fact has four statuses:
```JavaScript
0 - pending
1 - fulfilled with _value (_value is the internal property result we mentioned above)
2 - rejected with _value
3 - adopted the state of another promise, _value
```
## Chaining
`Promise.then` takes two callbacks: `res olve` and `reject` , and returns a promise
`Promise.catch(f)` is the same as `Promise.then(null, f)`
`Promise.finally(f)` is the same as `Promise.then(f, f)`, but f in `fanally` has **no argument**,
## `async` and `await`
They are syntax sugar for Promise.
## Promise API
`Promise.all`
`Promise.any`