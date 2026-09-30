# Function Binding (`bind`)

## Problem: `this` kho jaata hai
Object ka method alag se pass karo (jaise `setTimeout` me), to `this` object nahi rehta.

```javascript
let user = {
  firstName: "John",
  sayHi() { console.log(`Hello, ${this.firstName}!`); }
};

setTimeout(user.sayHi, 1000); // Hello, undefined!
```

## Solution 1: Wrapper
```javascript
setTimeout(() => user.sayHi(), 1000);
```
Kaam karta hai, lekin risk hai. Agar 1 second ke andar `user` badal gaya, to galat object ka method chalega.

## Solution 2: `bind` (behtar)
```javascript
let boundFunc = func.bind(context);
```
Ye ek naya function deta hai jisme `this` **hamesha ke liye fix** hota hai.

```javascript
let sayHi = user.sayHi.bind(user);
setTimeout(sayHi, 1000); // Hello, John!
```
Arguments jaise ke waise aage chale jaate hain, sirf `this` fix hota hai.

Kai methods ek saath bind karne ho to loop me kar sakte ho, ya lodash ka `_.bindAll`.

## Partial functions (arguments bhi fix karna)
```javascript
let bound = func.bind(context, arg1, arg2);
```
```javascript
function mul(a, b) { return a * b; }

let double = mul.bind(null, 2);
double(3); // 6
double(5); // 10
```
- `null` isliye diya kyunki `this` ki zarurat nahi, par `bind` ko kuch dena padta hai.
- Fayda: generic function se ek chhota, naam wala, aasan version ban jaata hai.

### `this` chhede bina sirf arguments fix karna
Native `bind` ye nahi karta, to khud helper likh sakte ho:
```javascript
function partial(func, ...argsBound) {
  return function (...args) {
    return func.call(this, ...argsBound, ...args);
  };
}
```

## Task se seekhi baatein
- **Bound function ka `this` badalta nahi.** `f.bind(a).bind(b)` karne par bhi `this = a` hi rehta hai.
- **Properties nahi aati.** `bind` naya object banata hai, to original function ki properties (`sayHi.test`) bound version me `undefined` hongi.
- **Callbacks me method dena ho:** `user.login.bind(user, true)` ya arrow function `() => user.login(true)`.

## Yaad rakho
| Tareeka | Note |
|---|---|
| Arrow wrapper | Simple, par outer variable badalne ka risk |
| `bind(obj)` | `this` pakka fix |
| `bind(obj, arg)` | `this` + pehle arguments fix (partial) |
