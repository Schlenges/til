# 'This' Keyword
### Global
- If 'this' is not defined in a declared object, its value is the *global object* (in case of the browser that would be the window object)
- In strict mode (ES5 "use strict") the value of 'this', when inside a function, is undefined (and not the global object)
  
### Implicit
- Inside of a declared object, the value of 'this' is the *closest parent object*
  
## Explicit Binding
### Call Method
```JS
function.call(thisArg, arg1, arg2, ...);
```
The parameter 'thisArg' defines what value 'this' should be. Every parameters following are the ones you want to pass to the function (allowed is an infinite amount).  
Example:
```JS
const person = {
  name: "Bonnie",
  sayHi: function(){return "Hi " + this.name}
}

const clyde = {name: "Clyde"}
person.sayHi.call(clyde)    // "Hi Clyde"
```

### Apply Method
```JS
function.apply(thisArg, [argsArray]);
```
Similiar to call(), but apply() takes only two parameters, the 'thisArg' and an array of parameters you want to pass to the function.  

**Call** and **apply** will immediately invoke the function they are called on.

### Bind Method
bind() is similiar to call(), only that it is not invoked immediately. It will return a new function definition with the value of the keyword 'this' explicitly set.  
You don't have to know all the parameters right away, but can use partial application.  
Example:
```JS
const person = {
  name: "Bonnie",
  sayHi: function(){return "Hi " + this.name},
  robberies: function(a, b, c, d){
    return this.name + " committed " + (a + b + c + d) + " robberies";
  }
}

const clyde = {name: "Clyde"};
const clydeRobberies = person.robberies.bind(clyde, 1, 2);
clydeRobberies(3, 4);  // Clyde committed 10 robberies
```

**Bind on the example of setTimeout()**  
setTimeout() is a method attached to the global window object, so using 'this' within it will not refer to the object it is declared in. Using bind() will solve this issue:
```JS
const person = {
	name: "Bonnie",
	sayHi: function(){
		setTimeout(function(){console.log("hi " + this.name)}.bind(this),1000) // .bind(this) binds the whole object to the function
	}
}

person.sayHi() // Hi Bonnie (1000ms later)
```
