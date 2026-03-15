```JS
const obj1 = { a: 1, b: 2 };
const obj2 = { b: 3, c: 4 };
const merged = Object.assign({}, obj1, obj2); // { a: 1, b: 3, c: 4 }, note this is shallow copy
```