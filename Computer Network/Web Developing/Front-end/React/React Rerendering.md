# General Behaviors:
For a functional/class component:
1. **Modifying variables in the components do not trigger rerender; modifying component states triggers rerender.**
    This is the main difference between normal variables in the components and component states.
2. **Parents’ rerender triggers children’s rerender.**
    Even if nothing needs to be changed in children.
3. **Change of primitive props triggers rerender, the existence of non-primitive props can trigger rerender.**
    Props changed is determined by **a shallow comparison** `prevProp === nextProp`
    Anonymous functions are considered non-primitive variables.
4. **A** **component calling** **`useContext`** **will rerender if the value of the context is changed.**
---
# Relevant Facts about Hooks:
1. memo(Child):
Disable **behavior 2** for Child
**DO NOT disable behavior 1**. React will still do the checking to see if the props of children have been changed.

2. useMemo, useCallback
useMemo(fn, deps): Circumvent non-primitive variables rerender in **behavior 3**
useCallback(fn, deps): equivalent to useMemo(() => fn, deps).
useMemo return the results of fn, useCallback returns the fn itself.
**NB:** useMemo and useCallback should only be used for performance optimization.

3. force render:
```JavaScript
// functional component
const [, forceUpdate] = useReducer((x) => x + 1, 0);
forceUpdate();
// class component
this.forceUpdate();
```
**NB:** should only be used for testing

4. useRef: 
	Purpose: 1. access DOM element 2. keep the value across rerendering
	Does not cause re-render

	Differences among useRef, variables in components and variables outside components in terms of render:
	[https://stackoverflow.com/questions/60554573/what-are-the-advantages-of-useref-instead-of-just-declaring-a-variable](https://stackoverflow.com/questions/60554573/what-are-the-advantages-of-useref-instead-of-just-declaring-a-variable)
	[https://stackoverflow.com/questions/57530446/difference-between-useref-and-normal-variable](https://stackoverflow.com/questions/57530446/difference-between-useref-and-normal-variable)

5. useEffect & useEffectLayout
    useEffect runs asynchronously **after** the render, therefore does not block the rendering
    useLayoutEffect runs synchronously **before** the render, therefore does block the rendering

    ```JavaScript
    useEffect(fn, []) // run once
    useEffect(fn) // always run
    ```

6. useContext
Context providers behave exactly like normal parent components in render. Thus, some argue that providers’s direct children should use `memo`.
---
## Gotchas
1. React strict mode renders components twice. Such behavior only exists in dev code, not in production.
2. “_A key part of this to understand is that “rendering” is not the same thing as “updating the DOM”, and a component may be rendered without any visible changes happening as a result.”_ However, _“React won’t update any DOM until one of the React components (re-)render.”_
---
# Main References
[https://alexsidorenko.com/blog/react-render-props/](https://alexsidorenko.com/blog/react-render-props/)