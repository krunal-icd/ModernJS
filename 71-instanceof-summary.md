# Class Checking: `instanceof`

## `instanceof` kya karta hai?
Check karta hai ki koi object kisi class ka hai ya nahi. **Inheritance bhi count hoti hai.**

```javascript
obj instanceof Class   // true / false
```

```javascript
class Animal {}
class Rabbit extends Animal {}
let rabbit = new Rabbit();

rabbit instanceof Rabbit; // true
rabbit instanceof Animal; // true (parent bhi)

[1, 2, 3] instanceof Array;  // true
[1, 2, 3] instanceof Object; // true
```
Constructor functions ke saath bhi chalta hai.

## Andar se kaise chalta hai?
1. Agar class me static method `Symbol.hasInstance` ho, to wahi call hota hai aur uska `true/false` final hota hai (custom logic ke liye).
2. Warna JS object ki **prototype chain** me `Class.prototype` dhoondhta hai:
```javascript
obj.__proto__ === Class.prototype ?
obj.__proto__.__proto__ === Class.prototype ?
... (chain ke end tak)
```
Kahin match mila to `true`, warna `false`.

Ek aur tareeka: `Class.prototype.isPrototypeOf(obj)` (same kaam).

**Ajeeb baat:** check me **constructor function khud shamil nahi hota**, sirf `Class.prototype` aur chain dekhi jaati hai. Isliye agar object banne ke baad `Rabbit.prototype` badal do, to `rabbit instanceof Rabbit` `false` ho jaata hai.

## Bonus: `Object.prototype.toString` se type
Ye `typeof` ka powerful version hai. Isse call karo `.call(value)` ke saath:
```javascript
let s = Object.prototype.toString;

s.call([]);        // [object Array]
s.call(123);       // [object Number]
s.call(null);      // [object Null]
s.call(alert);     // [object Function]
```

### `Symbol.toStringTag`
Is result ko customize kar sakte ho:
```javascript
let user = { [Symbol.toStringTag]: "User" };
{}.toString.call(user); // [object User]
```
Browser ke objects me (`window`, `XMLHttpRequest`) ye pehle se hota hai.

## Type checking ke tareeke
| Tareeka | Kis par chalta hai | Deta kya hai |
|---|---|---|
| `typeof` | primitives | string |
| `{}.toString` | primitives, built-in objects, `toStringTag` wale | string |
| `instanceof` | objects | true/false |

## Task: "Strange instanceof"
```javascript
function A() {}
function B() {}
A.prototype = B.prototype = {};
let a = new A();
a instanceof B; // true
```
Kyunki `instanceof` sirf **prototype** dekhta hai, function nahi. `a.__proto__ == B.prototype`, isliye `true`.

## Yaad rakho
- `instanceof` = class hierarchy check karne ke liye best.
- Built-in type string chahiye to `{}.toString.call(x)`.
- Type asal me prototype se decide hota hai, constructor se nahi.
