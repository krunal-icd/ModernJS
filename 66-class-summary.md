# Class Basic Syntax

## Basic syntax
Ek jaise bahut saare objects banane ke liye:
```javascript
class User {
  constructor(name) {
    this.name = name;
  }
  sayHi() {
    console.log(this.name);
  }
}

let user = new User("John");
user.sayHi();
```
- `new User(...)` par naya object banta hai aur `constructor` chalta hai.
- **Methods ke beech comma nahi lagta** (object literal se alag).

## Class asal me kya hai?
Class ek **function** hi hai.
```javascript
typeof User; // "function"
User === User.prototype.constructor; // true
```
`class` likhne par JS:
1. `User` naam ka function banata hai (code constructor se).
2. Saare methods `User.prototype` me daal deta hai.

## Sirf "syntactic sugar" nahi
Purane tareeke jaisa lagta hai, par kuch farak hain:
1. Class function bina `new` ke call nahi ho sakta (error).
2. Class ke methods **non-enumerable** hote hain (`for..in` me nahi aate).
3. Class ke andar ka code hamesha **strict mode** me chalta hai.

## Class Expression
Function ki tarah class bhi expression ban sakti hai, aur naam de sakte ho jo sirf class ke andar dikhta hai.
```javascript
let User = class MyClass { ... };
```
`makeClass(phrase)` jaisa function class return bhi kar sakta hai (on-demand classes).

## Getters / Setters aur computed names
Object literal ki tarah:
```javascript
class User {
  constructor(name) { this.name = name; }   // setter chalega
  get name() { return this._name; }
  set name(value) {
    if (value.length < 4) return;
    this._name = value;
  }
  ["say" + "Hi"]() { console.log("Hello"); } // computed name
}
```
Ye getters/setters `User.prototype` me bante hain.

## Class fields
Class me seedha property likh sakte ho:
```javascript
class User {
  name = "John";
}
```
Ye **har object par** set hoti hai, `User.prototype` par nahi.

### Fields se bound methods (this na khoye)
```javascript
class Button {
  constructor(value) { this.value = value; }
  click = () => { console.log(this.value); };
}
setTimeout(new Button("hello").click, 1000); // hello
```
Arrow function field har object ka apna function banata hai jisme `this` sahi rehta hai. Event listeners me bahut kaam aata hai.

## Yaad rakho
- Class = function + prototype methods.
- `new` ke bina call nahi hoti, strict mode me chalti hai.
- Fields object par set hoti hain, methods prototype par.
