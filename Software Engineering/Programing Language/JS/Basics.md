# **Data type**
`typeof variableName`
**Number**: 1, 109, infinity, -infinity, NaN
**BigInt**: 1234567890123456789012345678901234567890n
**String**:
Double quotes: `"Hello"`.
Single quotes: `'Hello'`.
Backticks: `` `Hello` ``.
**Boolean**: true, false
**null**: reference to a non-existing object
**undefined**: declared, but not assigned
**Object**
**Symbol**:
Symbol("description")
Symbol.for("description") // global
# **Type Conversion**
- Number(var), +var
    "c".codePointAt(0) // 99
- String(var)
    String.fromCodePoint(99) // c
- Boolean(var), !!var
- symbolVar.toString(), symbolVar.description
- JSON.stringify(obj)/JSON.parse(string)
    
    ```JavaScript
// old
exampleObject = {
	// for hint="string"
	toString() {
	return "string";
  },

  // for hint="number" or "default"
  valueOf() {
	return +number;
	}
}

//new
exampleObject = {
	[Symbol.toPrimitive](hint) {
	alert(`hint: ${hint}`);
	return hint == "string" ? `{name: "${this.name}"}` : this.money;
  }
}
    ```
    
exampleObject = {
	toString() {  
	return `{name: "${this.name}"}`;  
},  
  
// for hint="number" or "default"  
valueOf() {  
	return this.money;  
}  
  
# **alert, prompt and confirm**
```JavaScript
alert(title);
result = prompt(title, [default]);
result = confirm(question);
```