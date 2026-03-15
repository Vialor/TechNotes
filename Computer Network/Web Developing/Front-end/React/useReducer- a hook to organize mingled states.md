Consider this use case of useState (Ugly, especially when it makes more sense to merge `name` and `count` into a single object):
```TypeScript
const [count, setCount] = useState<number>(0)
const [name, setName] = useState<string>("");
const updateName = (newName: string) => {
	setCount(count+1)
	setName(newName)
};
const incrementCount = () => {
	setCount(count+1)
};
```
or when you merge `name` and `count` into a single object: (Better but still a bit messy, especially when the types of `action` gets more complex. For example, you would need to pass many callbacks like `updateName` and `incrementCount` to a child component, which would render your code really messy):
```TypeScript
const [{name, count}, setData] = useState<{name: string, count: number}>({
	count: 0,
	name: "",
});
const updateName = (newName: string) => {
	setData(d => {count: count+1, name: newName}
};
const incrementCount = () => {
	setData(d => {...d, count: count+1})
};
```
now we have useReducer(elegant):
```TypeScript
const reducer = (state, action) => {
	switch (action.type) {
		case "increament":
			return {...state, count: state.count+1}
		case "updateName":
			return {count: state.count+1, name: newName}
		default:
			return state
}};
const [{name, count}, dispatch] = useReducer(reducer, {
	count: 0,
	name: "",
});
// to use:
dispatch({type: "increment"})
```