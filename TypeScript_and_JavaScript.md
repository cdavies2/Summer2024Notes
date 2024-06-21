# TypeScript for JavaScript Programmers
* TypeScript offers all of JavaScript's features, along with TypeScript's type system.
* TypeScript checks if you've consistently assigned primitives like string and number.
* The main benefit of TypeScript is that it can highlight unexpected behavior in your code, lowering the chance of bugs.

## Types by Inference
* TypeScript knows the JavaScript language, so it will generate types for you in many cases. In creating a variable and assigning it to a particular value, TypeScript will use the value as its type
```
  let helloWorld= "Hello World"; // (TypeScript says let helloWorld: string)
```
* This offers a type-system without needing to add extra characters to make types explicit in your code.

## Defining Types
* You can use a wide variety of design patterns in JavaScript. Some design patterns make it difficult for types to be inferred automatically (EX: patterns that use dynamic programming). To cover these cases, TypeScript supports an extension of the JavaScript language, which offers places for you to tell TypeScript what the types should be
```
const user = {
  name: "Hayes",
  id: 0,
};
```
* You can also explicitly describe an object's shape using an interface declaration
```
interface User {
  name: string;
  id: number;
}
```
* You can then declare that a JavaScript object conforms to the shape of your new interface by using syntax like : TypeName after a variable declaration:
```
const user: User = {
  name: "Hayes",
  id: 0,
};
```
* If you provide an object that doesn't match the interface you've provided, TypeScript will warn you.
```
interface User {
  name: string;
  id: number;
}
const user: User = {
  username: "Hayes",
Object literal may only specify known properties, and 'username' does not exist in type 'User'.
  id: 0,
};
```
* You can use an interface declaration with classes:
```
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
* You can use interfaces to annotate parameters and return values to functions.
```
function deleteUser(user: User) {
  // ...
}
 
function getAdminUser(): User {
  //...
}
```
* Some TypeScript primitive types include...
    * any - allow anything
    * unknown - ensure someone using this type declares what the type is
    * never - it is not possible that this type could happen
    * void - a function which returns undefined or has no return value.
* There are two syntaxes for building types: Interfaces and Types. Only use type when you need specific features.

# Composing Types
* With TypeScript, you can create complex types by combining simple ones. You can do so with unions or with generics
## Unions
* With a union, you can declare that a type could be one of many types, like a boolean as being either true or false.
```
type MyBool = true | false;
```
* A popular use-case for union types is to describe the set of string or number literals that a value is allowed to be.
```
type WindowStates = "open" | "closed" | "minimized";
type LockStates = "locked" | "unlocked";
type PositiveOddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;
```
* Unions provide a way to handle different types too, like a function that takes an array or a string.
```
function getLength(obj: string | string[]) {
  return obj.length;
}
```
* To learn the type of a variable, use typof. For example, you can make a function return different values depending on whether it's passed a string or an array.
```
function wrapInArray(obj: string | string[]) {
  if (typeof obj === "string") {
    return [obj];
            
(parameter) obj: string
  }
  return obj;
}
```
## Generics
* Generics provide variables to types. A common example is an array; an array without generics could contain anything, but an array with generics can describe the values that the array contains.
```
type StringArray = Array<string>;
type NumberArray = Array<number>;
type ObjectWithNameArray = Array<{ name: string }>;
```
* You can also declare your own types that use generics:
```
interface Backpack<Type> {
  add: (obj: Type) => void;
  get: () => Type;
}
 
// This line is a shortcut to tell TypeScript there is a
// constant called `backpack`, and to not worry about where it came from.
declare const backpack: Backpack<string>;
 
// object is a string, because we declared it above as the variable part of Backpack.
const object = backpack.get();
 
// Since the backpack variable is a string, you can't pass a number to the add function.
backpack.add(23);
Argument of type 'number' is not assignable to parameter of type 'string'.
```
## Structural Type System
* In TypeScript, type checking focuses on the shape that values have. This is sometimes called "duck typing" or "structural typing."
* In a structural type system, if two objects have the same shape, they're considered to be of the same type.
```
interface Point {
  x: number;
  y: number;
}
 
function logPoint(p: Point) {
  console.log(`${p.x}, ${p.y}`);
}
 
// logs "12, 26"
const point = { x: 12, y: 26 };
logPoint(point);
```
* In the code above, the point variable is never declared to be a Point type, but TypeScript compares the shape of point to the shape of Point in the type-check. They have the same shape, so the code passes.
* If the object or class has all the required properties, TypeScript will say they match, regardless of the implementation details.
