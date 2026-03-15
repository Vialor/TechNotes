> functions that take one or more functions as arguments, or return a function as their result
# Closure
**Lexical scoping/static scoping**: the scope of a variable is determined by the code layout at the time of writing the program  
  
**Dynamic scoping**: the scope of a variable is determined at runtime, based on the calling sequence of functions
**Closure**: a function that captures some of the variables from the environment where it was created.  
Motivation: Return the function defined inside another function, which has access to the outer function's variables. Even when the outer function returns, the inner function still have keep variables of outer functions in memory. This gives us a stateful function.  
# Callback (JS)

> **Callback**: a function that is passed as an argument to other functions  
> Motivation: the function passed can be called ("called back") at some later point in time  
```JavaScript
function resolveAfter2Seconds() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve('resolved');
    }, 2000);
  });
}
async function asyncCall() {
  console.log('calling');
  const result = await resolveAfter2Seconds();
  console.log(result);
  // Expected output: "resolved"
}
asyncCall();
```
# Decorator (in Python)
```Python
def decoratorWithParameter(username):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            print(username + " is calling")
            returnValue = func(*args, **kwargs)
            print(username + "has done calling")
            return returnValue
        return wrapper
    return decorator

@decoratorWithParameter("Alice")
def example_function():
	pass
# same as: example_function = decoratorWithParameter("Alice")(example_function)
# @wrap propagates the wrapper's attributes to the wrapped example_function including example_function.__name__ and example_function.__doc__
```
# Currying
Motivation: avoid passing the same variable multiple times, and help create a higher order function
```Python
def change(a):
    def w(b):
        def x(c):
            def y(d):
                def z(e):
                    print(a, b, c, d, e)
                return z
            return y
        return x
    return w
change(10)(20)(30)(40)(50)
```