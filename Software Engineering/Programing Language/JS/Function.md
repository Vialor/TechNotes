# **JS - Function**
## **Function declaration VS Function expression**
declaration :
`function f() {} // no semicolon`
expression:
`const f= function() {};`
`const f= () => {};`
**Function expressions are used to avoid being globally usable.**
### **Hoisting**
declaration: yes
```Plain
f();
function doStuff() {}
```
expression: error
```Plain
f();
const f = () => {};
```
### **IIFE**
**immediately invoked function expressions**
`((parameters) => {})();`
### **Callback Function**
functions being used as parameters in other functions