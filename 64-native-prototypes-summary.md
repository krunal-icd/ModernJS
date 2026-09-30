# Native Prototypes

Built-in cheezein (Object, Array, Date, Function...) bhi isi `prototype` system par bani hain.

## `Object.prototype`
`let obj = {}` asal me `new Object()` jaisa hai. Iska prototype `Object.prototype` hota hai, jisme `toString`, `hasOwnProperty` jaise methods hote hain. Isliye khaali object par bhi `obj.toString()` chal jaata hai.

```javascript
let obj = {};
obj.__proto__ === Object.prototype;   // true
Object.prototype.__proto__;           // null (chain yahin khatam)
```

## Baaki built-in prototypes
`Array`, `Date`, `Function` sabke methods unke prototype me hote hain. Data alag hota hai (array ke items), methods shared, isliye memory bachti hai.

Sabke top par `Object.prototype` hota hai, isliye "sab kuch object se inherit karta hai".

```javascript
let arr = [1, 2, 3];
arr.__proto__ === Array.prototype;                 // true
arr.__proto__.__proto__ === Object.prototype;      // true
arr.__proto__.__proto__.__proto__;                 // null
```
Agar do prototypes me same naam ka method ho (jaise `toString`), to **nazdeek wala** jeetta hai (`Array.prototype.toString`).

## Primitives
String, number, boolean object nahi hote. Par unki property access karte hi ek **temporary wrapper object** (`String`, `Number`, `Boolean`) ban jaata hai jo methods deta hai, phir gayab. `null` aur `undefined` ke wrapper nahi hote.

## Native prototype badalna
```javascript
String.prototype.show = function () { console.log(this); };
"BOOM!".show();
```
Ye kaam karta hai, lekin **aam taur par buri aadat** hai. Prototypes global hote hain. Do libraries same naam ka method jodengi to ek doosre ko overwrite kar dengi.

**Ek hi sahi case: polyfill.** Jab koi standard method browser me nahi hai, tab check karke jod do.
```javascript
if (!String.prototype.repeat) {
  String.prototype.repeat = function (n) { /* ... */ };
}
```

## Prototype se method udhaar lena
Array jaisa object (index + `length`) ho to Array ka method udhaar le sakte ho.
```javascript
let obj = { 0: "Hello", 1: "world!", length: 2 };
obj.join = Array.prototype.join;
obj.join(","); // "Hello,world!"
```
Ye isliye chalta hai kyunki `join` sirf index aur `length` dekhta hai.

## Yaad rakho
- Methods prototype me, data object me.
- Native prototypes ko na chhedo (sirf polyfill ke liye theek).
- Method borrowing ek kaam ka trick hai.
