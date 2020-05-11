# Basic Types

## Array
Array types can be written in one of two ways. In the first, you use the type of the elements followed by [] to denote an array of that element type:
```TS
let list: number[] = [1, 2, 3];
```
The second way uses a generic array type:
```TS
let list: Array<number> = [1, 2, 3];
```

## Tuple
Tuple types allow you to express an array with a fixed number of elements whose types are known, but need not be the same. For example, you may want to represent a value as a pair of a string and a number:
```TS
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ["hello", 10]; // OK
// Initialize it incorrectly
x = [10, "hello"]; // Error
```

## Enum
A helpful addition to the standard set of datatypes from JavaScript is the enum. As in languages like C#, an enum is a way of giving more friendly names to sets of numeric values.
```TS
enum Color {
  Red,
  Green,
  Blue,
}
let c: Color = Color.Green;
```
By default, enums begin numbering their members starting at 0. You can change this by manually setting the value of one of its members. For example, we can start the previous example at 1 instead of 0:
```TS
enum Color {
  Red = 1,
  Green,
  Blue,
}
let c: Color = Color.Green;
```
Or, even manually set all the values in the enum:
```TS
enum Color {
  Red = 1,
  Green = 2,
  Blue = 4,
}
let c: Color = Color.Green;
```
A handy feature of enums is that you can also go from a numeric value to the name of that value in the enum. For example, if we had the value 2 but weren’t sure what that mapped to in the Color enum above, we could look up the corresponding name:
```TS
enum Color {
  Red = 1,
  Green,
  Blue,
}
let colorName: string = Color[2];

console.log(colorName); // Displays 'Green' as its value is 2 above
```

## Any
We may need to describe the type of variables that we do not know when we are writing an application. These values may come from dynamic content, e.g. from the user or a 3rd party library. In these cases, we want to opt-out of type checking and let the values pass through compile-time checks. To do so, we label these with the ```any``` type. The any type is a powerful way to work with existing JavaScript, allowing you to gradually opt-in and opt-out of type checking during compilation.  
The any type is also handy if you know some part of the type, but perhaps not all of it. For example, you may have an array but the array has a mix of different types:
```TS
let list: any[] = [1, true, "free"];

list[1] = 100;
```

## Void

## Null and Undefined
In TypeScript, both undefined and null actually have their own types named undefined and null respectively. 

By default null and undefined are subtypes of all other types. That means you can assign null and undefined to something like number.  
However, when using the ```--strictNullChecks``` flag, null and undefined are only assignable to ```any``` and their respective types (the one exception being that undefined is also assignable to void). This helps avoid many common errors. In cases where you want to pass in either a string or null or undefined, you can use the union type ```string | null | undefined.```

## Never
The never type represents the type of values that never occur. For instance, never is the return type for a function expression or an arrow function expression that always throws an exception or one that never returns. Variables also acquire the type never when narrowed by any type guards that can never be true.
The never type is a subtype of, and assignable to, every type; however, no type is a subtype of, or assignable to, never (except never itself). Even any isn’t assignable to never.  
Some examples of functions returning never:
```TS
// Function returning never must have unreachable end point
function error(message: string): never {
  throw new Error(message);
}

// Inferred return type is never
function fail() {
  return error("Something failed");
}

// Function returning never must have unreachable end point
function infiniteLoop(): never {
  while (true) {}
}
```

## Type assertions
Sometimes you’ll end up in a situation where you’ll know more about a value than TypeScript does. Usually this will happen when you know the type of some entity could be more specific than its current type.

Type assertions are a way to tell the compiler “trust me, I know what I’m doing.” A type assertion is like a type cast in other languages, but performs no special checking or restructuring of data. It has no runtime impact, and is used purely by the compiler. TypeScript assumes that you, the programmer, have performed any special checks that you need.

Type assertions have two forms. One is the “angle-bracket” syntax:
```TS
let someValue: any = "this is a string";

let strLength: number = (<string>someValue).length;
```
And the other is the as-syntax:
```TS
let someValue: any = "this is a string";

let strLength: number = (someValue as string).length;
```
The two samples are equivalent. Using one over the other is mostly a choice of preference; however, when using TypeScript with JSX, only as-style assertions are allowed.

(source: https://www.typescriptlang.org/docs/handbook/basic-types.html)