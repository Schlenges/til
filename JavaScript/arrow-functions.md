# Arrow Functions
```JS
const myFunc = () => {
  const myVar = "value";
  return myVar;
}
```
If the function has only a return value, arrow function syntax allows you to omit the keyword 'return' as well as the brackets surrounding the code. This helps simplify smaller functions into one-line statements:
```JS
const myFunc= () => "value"
```

Just like a normal function, you can pass arguments into arrow functions. If there is only one parameter, the parentheses can be omitted from the definition:
```JS
const doubler = item => item * 2;
```