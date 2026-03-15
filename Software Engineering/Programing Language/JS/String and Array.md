# String
get char: str[pos]
insert with unicode: \u0000
## Quotes
Double quotes: `"Hello"`.
Single quotes: `'Hello'`.
Backticks: `` `Hello` ``.
- String Interpolation
    
    `` `Hello, ${name}!` ``
    
- Multi-line
    
    ```HTML
    `multiple
    lines
    with
    backticks
    `
    "multiple\nlines\nwith\ndouble\nquotes"
    ```
    
  
## Methods and Properties
`.length`
  
`.toLowerCase/toUpperCase()`
`.indexOf(str, from)` `.lastIndexOf(item, from)`
`.startsWith(str)/endsWith(str)/includes(str, num)`
`.trim()`
`.repeat(times)`
`.slice(start, [end])`
  
`.localeCompare(compareString[, locales])`
# Array
get item: arr[pos]
## Methods and Properties
`.indexOf(item, from)` `.lastIndexOf(item, from)`
`.includes(item, from)`
  
`.shift()`
`.unshift()`
`.pop()`
`.push()`
  
`.length`
  
`.splice(start[, deleteCount, elem1, ..., elemN])`
`.slice([start], [end])`
`.concat(arg1, arg2...)`
  
## Going through
```JavaScript
for (let v of arr) {}
for (let k in obj) {}
arr.forEach(function(item, index, array) {});
```
  
## Advanced
```JavaScript
arr.find(function(item[, index[, array]])) {
  // if true is returned, item is returned and iteration is stopped
  // for falsy scenario returns undefined
});
arr.findIndex(function(item[, index[, array]]))
arr.sort(function(a, b) {
  // return: a positive number-"greater"-a after b; a negative number-"less"-b after a
});
arr.reverse();
arr.filter(function(item, index, array) {
  // if true item is pushed to results and the iteration continues
  // returns empty array if nothing found
});
arr.map(function(item, index, array) {
  // returns the new value instead of item
});
arr.reduce(function(accumulator, item, index, array) {
}, [initial]);
arr.reduceRIght(function(accumulator, item, index, array) {
}, [initial]);
arr.some(fn)
arr.every(fn)
arr.fill(value[, start[, end]])
```
```JavaScript
arr.sort(function(a, b) {
  // return: a positive number-"greater"-a after b; a negative number-"less"-b after a
});
arr.reverse();
arr.filter(function(item, index, array) {
  // if true item is pushed to results and the iteration continues
  // returns empty array if nothing found
});
arr.map(function(item, index, array) {
  // returns the new value instead of item
});
arr.reduce(function(accumulator, item, index, array) {
}[, initial]);
arr.reduceRIght(function(accumulator, item, index, array) {
}[, initial]);
arr.some(fn)
arr.every(fn)
arr.fill(value[, start[, end]])
```
  
# Other
```JavaScript
arr = str.split(delim)
str = arr.join(glue)
f
Array.isArray(arr)
```
  
## Smart Code
```JavaScript
function removeDuplicates(array) {
	return array.filter((a, b) => array.indexOf(a) === b)
}; // though the best complexity is nlogn
```
  
## Array-like and Iterable
Iterable: objects that implement the `Symbol.iterator` (i.e. objects that can be for...of'ed)
Array-like: objects that have indexes and `length`
`Array.from(obj[, mapFn, thisArg])`: turn iterable and array-like into real array
  
```HTML
iterable = {
	[Symbol.iterator]: function(){
		return {
			next() {
				return {done, value}
			}
		}
	}
}
iterable = {
	[Symbol.iterator]: function(){
		return {
			this
		}
	}
	next() {
				return {done, value}
	}
}
```