# Data Types – Simple Hinglish Summary

Source: https://javascript.info/types

## Main baat
JavaScript me har value ka ek **type** hota hai (jaise string ya number). JS me **8 basic data types** hain. Ye chapter unka general intro hai, detail aage ke chapters me aayegi.

### Dynamically typed language
Variable me **kisi bhi type ki value** rakh sakte ho, aur baad me type badal bhi sakte ho. Ise **"dynamically typed"** kehte hain.

```javascript
let message = "hello";
message = 123456;   // koi error nahi
```

## 1. Number
Integer aur decimal (floating point), dono ke liye ek hi type.

```javascript
let n = 123;
n = 12.345;
```

Operations: `*`, `/`, `+`, `-` etc.

### Special numeric values
- **`Infinity`** → mathematical infinity, har number se bada. Zero se divide karne par milta hai.
  ```javascript
  alert( 1 / 0 );      // Infinity
  ```
- **`NaN`** (Not a Number) → computational error. Galat math operation ka result.
  ```javascript
  alert( "not a number" / 2 );   // NaN
  ```
  **NaN "sticky" hota hai:** uske saath koi bhi math karo, result NaN hi rahega.
  ```javascript
  alert( NaN + 1 );   // NaN
  alert( 3 * NaN );   // NaN
  ```
  (Ek hi exception: `NaN ** 0` = `1`)

### Math "safe" hai
JS me zero se divide karna jaisi galtiyon par script **crash nahi hoti.** Worst case me `NaN` milta hai.

## 2. BigInt
- Number type **`±(2^53 - 1)`** (yaani `9007199254740991`) tak ke integers safely store karta hai. Isse bade numbers me **precision error** aata hai.
  ```javascript
  console.log(9007199254740991 + 1); // 9007199254740992
  console.log(9007199254740991 + 2); // 9007199254740992  (same!)
  ```
- **BigInt** bahut bade integers (kisi bhi length ke) ke liye hai, jaise cryptography ke liye.
- Integer ke end me **`n`** lagao:
  ```javascript
  const bigInt = 1234567890123456789012345678901234567890n;
  ```
- Kam use hota hai, alag chapter me detail hai.

## 3. String
String ko **quotes** me likhna zaruri hai. **3 tarah ke quotes:**

| Quote | Example | Khaasiyat |
|-------|---------|-----------|
| Double `" "` | `"Hello"` | Simple quote |
| Single `' '` | `'Hello'` | Simple quote (double jaisa hi) |
| Backtick `` ` ` `` | `` `Hello` `` | **Extended functionality** |

**Backticks me variable/expression embed** kar sakte ho `${...}` se:

```javascript
let name = "John";

alert( `Hello, ${name}!` );          // Hello, John!
alert( `the result is ${1 + 2}` );   // the result is 3
```

Ye **sirf backticks me** kaam karta hai. Double quotes me nahi:

```javascript
alert( "the result is ${1 + 2}" );   // the result is ${1 + 2}
```

**Note:** JS me alag se **character type nahi hai** (C/Java jaisa `char`). Sirf `string` hai, chahe 0, 1 ya zyada characters ho.

## 4. Boolean
Sirf **do values:** `true` aur `false`. Yes/no ke liye use hota hai.

```javascript
let nameFieldChecked = true;
let ageFieldChecked = false;

let isGreater = 4 > 1;
alert( isGreater );   // true
```

Comparison ka result bhi boolean hota hai.

## 5. `null`
- Ye ek **alag type** hai jisme sirf ek value `null` hai.
- Matlab: **"kuch nahi", "empty" ya "value unknown".**
- Ye "non-existing object ka reference" nahi hai (dusri languages ki tarah).

```javascript
let age = null;   // age unknown hai
```

## 6. `undefined`
- Ye bhi **alag type** hai. Matlab: **"value assign nahi hui."**
- Variable declare kiya lekin value nahi di, to value `undefined` hoti hai.

```javascript
let age;
alert(age);   // "undefined"
```

- Explicitly `undefined` assign kar sakte ho, lekin **recommend nahi.**
- **Rule:** "empty/unknown" dikhane ke liye **`null`** use karo. `undefined` sirf unassigned cheezon ke default ke liye rakho.

## 7. Object aur Symbol
- **Object:** Baaki sab types **primitive** hain (ek hi cheez store karte hain). Object **collections of data** aur complex cheezein store karta hai. Detail baad me.
- **Symbol:** Objects ke liye **unique identifiers** banane ke liye. Detail baad me.

## 8. `typeof` operator
Value ka type batata hai (string ke roop me).

```javascript
typeof undefined      // "undefined"
typeof 0              // "number"
typeof 10n            // "bigint"
typeof true           // "boolean"
typeof "foo"          // "string"
typeof Symbol("id")   // "symbol"
typeof Math           // "object"
typeof null           // "object"   <-- language ki galti
typeof alert          // "function"
```

### Dhyan dene wali baatein
- **`typeof null` = `"object"`:** Ye JS ki **purani officially recognized galti** hai, compatibility ke liye rakhi gayi hai. `null` object **nahi hai.**
- **`typeof alert` = `"function"`:** Functions asal me **object type** ke hi hain, lekin `typeof` unhe alag dikhata hai (practical hai).
- **`typeof x` aur `typeof(x)`** same hain. `typeof` ek **operator** hai, function nahi. Brackets sirf grouping ke hain. `typeof x` zyada common hai.

## Summary

**8 data types:**

**7 Primitive:**
| Type | Kis liye |
|------|----------|
| `number` | Integer aur decimal (limit: `±(2^53-1)`) |
| `bigint` | Kisi bhi length ke integers |
| `string` | Text (alag char type nahi) |
| `boolean` | `true` / `false` |
| `null` | Unknown / empty value |
| `undefined` | Unassigned value |
| `symbol` | Unique identifiers |

**1 Non-primitive:**
| Type | Kis liye |
|------|----------|
| `object` | Complex data structures |

## Practice Task
**String quotes ka output kya hoga?**

```javascript
let name = "Ilya";

alert( `hello ${1}` );        // hello 1
alert( `hello ${"name"}` );   // hello name
alert( `hello ${name}` );     // hello Ilya
```

Backticks me `${...}` ke andar ka expression evaluate hokar string me judta hai.
