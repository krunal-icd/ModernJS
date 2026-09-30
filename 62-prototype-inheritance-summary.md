# Prototypal Inheritance

## Idea
Ek object ko doosre object ki cheezein reuse karni ho to copy karne ki jagah usse **jod** do. Ye jodna `[[Prototype]]` se hota hai.

## `[[Prototype]]`
Har object ke paas ek hidden property `[[Prototype]]` hoti hai. Ye ya to `null` hoti hai ya kisi doosre object ko point karti hai. Use set karne ka ek tareeka `__proto__` hai.

```javascript
let animal = { eats: true, walk() { console.log("Animal walk"); } };
let rabbit = { jumps: true, __proto__: animal };

rabbit.eats; // true (animal se mila)
rabbit.walk(); // Animal walk
```
Property object me na mile to JS **prototype me dhoondhta hai**, phir uske prototype me, aise upar tak (prototype chain).

Rules:
- Chain gol nahi ghoom sakti (error).
- `__proto__` sirf object ya `null` hi ho sakta hai.
- Ek object ka **sirf ek** `[[Prototype]]` hota hai.
- `__proto__` purana getter/setter hai. Modern tareeka `Object.getPrototypeOf` / `Object.setPrototypeOf` hai.

## Likhna prototype use nahi karta
Prototype sirf **padhne** ke liye hai. Likhna/delete karna seedha object par hota hai.
```javascript
rabbit.walk = function () { console.log("Rabbit bounce!"); };
// ab rabbit ka apna walk hai, animal wala nahi chhua
```
Exception: agar prototype me **setter** hai to assignment par wahi setter chalta hai.

## `this` prototype se affect nahi hota
Method kahin se bhi mile, `this` hamesha **dot se pehle wala object** hota hai.
```javascript
let animal = { sleep() { this.isSleeping = true; } };
let rabbit = { __proto__: animal };

rabbit.sleep();
rabbit.isSleeping;  // true
animal.isSleeping;  // undefined
```
Yaani **methods shared hain, par state (data) shared nahi.**

## `for..in`
`for..in` apni **aur inherited** dono properties dikhata hai. `Object.keys`, `Object.values` sirf apni. Inherited hatane ke liye `obj.hasOwnProperty(key)`.

## Task se seekhi baat: dono hamsters ka pet kyun bhara?
`stomach: []` prototype me rakha aur `this.stomach.push(...)` kiya, to push prototype wale array me hua. Sabka pet ek hi ho gaya.

Fix: har object ki apni state usi object me rakho (`stomach: []` har hamster me), ya `this.stomach = [food]` jaisa assignment karo.

## Yaad rakho
- Padhna: chain me dhoondho. Likhna: seedha object par.
- `this` = dot se pehle wala object.
- State apne object me, methods prototype me.
