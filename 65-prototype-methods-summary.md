# Prototype Methods aur `__proto__` ke bina objects

## Modern tareeke
`obj.__proto__` purana hai. Behtar:

| Kaam | Method |
|---|---|
| Prototype padhna | `Object.getPrototypeOf(obj)` |
| Prototype set karna | `Object.setPrototypeOf(obj, proto)` |
| Given prototype se naya object | `Object.create(proto, descriptors)` |

Literal me `{ __proto__: animal }` likhna theek maana jaata hai.

```javascript
let animal = { eats: true };
let rabbit = Object.create(animal);   // {__proto__: animal} jaisa
Object.getPrototypeOf(rabbit) === animal; // true
```

`Object.create` ka doosra argument property descriptors leta hai:
```javascript
let rabbit = Object.create(animal, { jumps: { value: true } });
```

### Poora exact clone
```javascript
let clone = Object.create(
  Object.getPrototypeOf(obj),
  Object.getOwnPropertyDescriptors(obj)
);
```
Isme saari properties (enumerable/non-enumerable, getters/setters) aur sahi prototype aa jaate hain.

## Thoda itihaas
- Sabse purana: `F.prototype`.
- 2012: `Object.create`.
- 2015: `getPrototypeOf` / `setPrototypeOf` aaye. `__proto__` ko purana maana gaya.
- 2022: object literal me `__proto__` officially allowed, par `obj.__proto__` getter/setter abhi bhi purana hi hai.

**Speed ka dhyan:** existing object ka prototype baad me badalna bahut **slow** hota hai. Prototype ek baar creation ke waqt set karo.

## "Bahut plain" objects (dictionary)
Agar user ki di hui keys object me store karte ho, to `"__proto__"` key ek problem hai. Kyunki `__proto__` sirf object/null accept karta hai, string assign karne par ignore ho jaata hai.

```javascript
let obj = {};
obj["__proto__"] = "some value";
obj["__proto__"]; // [object Object], "some value" nahi!
```
`__proto__` asal me `Object.prototype` ka accessor hai, isliye ye hota hai.

**Solutions:**
1. `Map` use karo (sabse safe).
2. `Object.create(null)` se aisa object banao jiska prototype hi nahi:
```javascript
let dict = Object.create(null);
dict["__proto__"] = "some value"; // ab normal key jaisa chalega
```
Aise object me `toString` jaise built-in methods nahi hote (ye aksar theek hai), lekin `Object.keys(dict)` jaise `Object.xyz()` functions chalte hain.

## Task se seekhi baat
```javascript
rabbit.sayHi();                       // this = rabbit -> naam dikhega
Rabbit.prototype.sayHi();             // this = Rabbit.prototype -> undefined
Object.getPrototypeOf(rabbit).sayHi();// undefined
```
`this` hamesha dot se pehle wala object hota hai.

## Yaad rakho
- `Object.create`, `getPrototypeOf`, `setPrototypeOf` use karo.
- `Object.create(null)` = pure dictionary object.
- Prototype baar-baar mat badlo.
