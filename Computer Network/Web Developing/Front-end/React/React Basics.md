# From HTML to React
## DOM Document Object Model
**DOM tree**: a tree structure representing an HTML or XML document wherein each node is an object representing an element
**DOM**: A cross-platform and language-independent interface that manipulates the **DOM Tree** to modify underlying the HTML or XML document (built-in at the browser env)
## AJAX Asynchronous JavaScript & XML

> HTML forms and AJAX are the main two ways to send http requests in HTML

**XML eXtensible Markup Language**: Storing, transporting, and sharing structured data between systems and applications.  
Now we usually use JSON instead, XML is here for historical reason.  
**AJAX**: It's a set of web development techniques that allows web applications to communicate with a server and retrieve data asynchronously, without having to reload the whole web page.
2 Options to achieve AJAX: (They are built-in at the browser env)
1. XMLHttpRequest (jQuery is based on this)
2. Fetch API, with its promise-based structure (Axios is based on Fetch)
## JSX JavaScript XML
JSX is a language extension to JavaScript language; it is a syntax sugar
```JavaScript
(<div>
	<h1>Title</h1>
	<div>Content</div>
	{react code that returns a JSX}
</div>)

const element = <h1>Hello, world!</h1>;
// The above line will be compiled into the following by Babel (depends on compiler)
const element = React.createElement('h1', null, 'Hello, world!');
```
## React
React uses DOM API under the hood. React is written with ES6 + JSX.
**Babel** is a compiler that translate React codes into plain JS so browsers can understand
```HTML
<div id="root"></div>
	
-->

<script type="text/babel">
	const root = ReactDOM.createRoot(document.getElementById("root"));
	root.render(<App />)
</script>
```
### Virtual DOM
A virtual DOM tree that only exists in memory; it is used to minimize the operation to the real DOM tree.
在每次状态或数据变化时，React 会更新 Virtual DOM 树。积攒一定的更新数量后做批量更新（**batching**）：将更新后的 Virtual DOM 与之前的虚拟 DOM 进行比较（**diffing**），找出需要修改的部分，最后只把发生变化的部分应用到真实的 DOM 中（**reconciliation**）。
# Components
## Class Component
```JSX
class App extends React.Component {
	constructor() {
		super();
		this.state() = {
			letters = ['a', 'b', 'c']
		}
	}
	
	componentDidMount() {
		this.setState({
			letters = ['A', 'B', 'C']
		})
	}
	
	render() {
		return (<div>
			<h1>Title</h1>
			<div>Content</div>
			{react code that returns a JSX}
		</div>)
	}
}
```
## Functional Component
```JSX
import React from 'react';
const MyComponent = (props) => {
	return <div>Hello, {props.name}!</div>;
};
export default MyComponent;
```