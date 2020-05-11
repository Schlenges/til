# Overview

- Types by **Inference**  

## Interfaces:
An explicit way to describe this object’s shape is via an interface declaration:
  ```TS
  interface User {
    name: string;
    id: number;
  }
  ```  
  You can then declare that a JavaScript object conforms to that shape of your new interface by using syntax like : TypeName after a **variable declaration**:
  ```TS
  const user: User = {
    name: "Hayes",
    id: 0,
  }
  ```  
  An interface declaration can also be used with **classes**:
  ```TS
  interface User {
    name: string;
    id: number;
  }

  class UserAccount {
    name: string;
    id: number;

    constructor(name: string, id: number) {
      this.name = name;
      this.id = id;
    }
  }

  const user: User = new UserAccount("Murphy", 1);
  ```  
  Interfaces can also be used to annotate **parameters** and **return values** to functions:
  ```TS
  function getAdminUser(): User {
    //...
  }

  function deleteUser(user: User) {
    // ...
  }
  ```  

  TypeScript extends the list of primitive types available in JavaScript with a few more. There are two syntaxes for building types: Interfaces and Types - you should prefer ```interface```, and use ```type``` when you need specific features.

## Composing Types:
### Unions
A union is a way to declare that a type could be one of many types. For example, you could describe a boolean type as being either true or false:
```TS
type MyBool = true | false
```  

One of the most popular use-cases for union types is to describe a set of strings or numbers literal which a value is allowed to be:
```TS
type WindowStates = "open" | "closed" | "minimized";
type LockStates = "locked" | "unlocked";
type OddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;
```  
Unions provide a way to handle different types too, for example you may have a function which accepts an array or a string.
```TS
function getLength(obj: string | string[]) {
  return obj;
}
```  

### Generics
Generics are a way to provide variables to types.  
A common example is an array, an array without generics could contain anything. An array with generics can describe what values are inside in the array.
```TS
type StringArray = Array<string>;
type NumberArray = Array<number>;
type ObjectWithNameArray = Array<{ name: string }>;
```
You can declare your own types which use generics:
```TS
interface Backpack<Type> {
  add: (obj: Type) => void;
  get: () => Type;
}
```  

### Structural Type System
One of TypeScript’s core principles is that type checking focuses on the shape which values have. This is sometimes called “duck typing” or “structural typing”. In a structural type system if two objects have the same shape, they are considered the same.
```TS
interface Point {
  x: number;
  y: number;
}

function printPoint(p: Point) {
  console.log(`${p.x}, ${p.y}`);
}

// prints "12, 26"
const point = { x: 12, y: 26 };
printPoint(point);
```
The point variable is never declared to be a Point type, but TypeScript compares the shape of point to the shape of Point in the type-check. Because they both have the same shape, then it passes.

**The shape matching only requires a subset of the object’s fields to match.**
Structurally there is no difference between how classes and objects conform to shapes. If the object or class has all the required properties, then TypeScript will say they match regardless of the implementation details.

(source: https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html)