## **The request lifecycle**
![[Untitled 10.png|Untitled 10.png]]
![[Untitled 1 3.png|Untitled 1 3.png]]
- Incoming request
- Globally bound middleware
- Module bound middleware
- Global guards
- Controller guards
- Route guards
- Global interceptors (pre-controller)
- Controller interceptors (pre-controller)
- Route interceptors (pre-controller)
- Global pipes
- Controller pipes
- Route pipes
- Route parameter pipes
- Controller (method handler)
- Service (if exists)
- Route interceptor (post-request)
- Controller interceptor (post-request)
- Global interceptor (post-request)
- Exception filters (route, then controller, then global)
- Server response
---
  
## What is it and What does it do
Exception filters: processing all unhandled exceptions across an application
  
Middleware: functions called before route handler
Apply changes to the **request** and/or the **response** objects
End the request-response cycle or **redirect** to another route.
Must call the **next()** middleware in the queue if it didn't redirect or ended the response cycle.
  
Guards: A guard is a class annotated with the `@Injectable()` decorator, which implements the `CanActivate` interface.
Guards have a single responsibility, **Authorization**. They either allow a request to be handled by a route handler or not. As their name implies, they protect our routes handlers.
  
Pipes: A pipe is a class annotated with the `@Injectable()`decorator, which implements the `PipeTransform` interface.
Data Validation: When it is not valid - throw an exception; If valid - pass it through unchanged
Transformation: of data to the desired type
  
Interceptors: An interceptor is a class annotated with the `@Injectable()` decorator, which implements the `NestInterceptor`  
 interface.  
Bind extra logic before / after method execution Transform the result returned from a function
Transform the exception thrown from a function
Extend the basic function behavior
Completely override a function depending on specific conditions (e.g., for caching purposes)
  
  
Controller: handling incoming **requests** and returning **responses** to the client
  
Providers: can be **injected** as a dependency to any pipeline components above
---