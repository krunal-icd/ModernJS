# Variables – Simple Hinglish Summary

Source: https://javascript.info/variables

## Main baat
JavaScript application ko zyadatar **information** ke saath kaam karna padta hai. Jaise online shop me products aur cart, ya chat app me users aur messages. Is information ko store karne ke liye **variables** use hote hain.

## 1. Variable kya hai?
- Variable ek **"named storage"** hai, yaani data ka naam wala dabba.
- Variable banane ke liye **`let`** keyword use karte hain.

```javascript
let message;              // variable declare kiya
message = 'Hello';        // value store ki (= assignment operator)
```

- Declare aur assign ek hi line me bhi kar sakte ho:

```javascript
let message = 'Hello!';
alert(message); // Hello!
```

- **Ek line me multiple variables** bhi ban sakte hain, lekin **recommend nahi.** Readability ke liye **ek line me ek variable** likho:

```javascript
// recommended
let user = 'John';
let age = 25;
let message = 'Hello';
```

### `var` vs `let`
Purane code me `var` milta hai. Ye `let` jaisa hi hai lekin **"old-school"** tarike se. Abhi use nahi karte. Iske differences alag chapter ("The old var") me aayenge.

## 2. Real-life analogy
Variable ko ek **dabba (box)** samjho jis par **naam ka sticker** laga hai. Dabbe me koi bhi value rakh sakte ho aur **kitni bhi baar badal sakte ho.** Value badalne par purana data hat jata hai.

```javascript
let message;
message = 'Hello!';
message = 'World!';   // value change ho gayi
alert(message);       // World!
```

**Ek variable se dusre me copy:**

```javascript
let hello = 'Hello world!';
let message;
message = hello;      // copy

alert(hello);   // Hello world!
alert(message); // Hello world!
```

### Do baar declare karna error hai
Variable ko **sirf ek baar** declare karo. Dobara `let` likhne par error aata hai:

```javascript
let message = "This";
let message = "That"; // SyntaxError: 'message' has already been declared
```

Dobara use karna ho to bina `let` ke sirf naam likho.

### Functional languages (extra info)
Haskell jaisi languages me variable ki value **change nahi kar sakte.** Nayi value ke liye naya variable banana padta hai. Phir bhi ye serious kaam ke liye capable hain.

## 3. Variable naming rules
**Do limitations:**
1. Naam me sirf **letters, digits, `$` aur `_`** ho sakte hain.
2. Pehla character **digit nahi** hona chahiye.

```javascript
let userName;    // valid
let test123;     // valid
let $ = 1;       // valid
let _ = 2;       // valid

let 1a;          // galat, digit se shuru nahi ho sakta
let my-name;     // galat, hyphen (-) allowed nahi
```

- **camelCase** use karo (multiple words ke liye): `myVeryLongName`
- **Case matter karta hai:** `apple` aur `APPLE` do alag variables hain.
- **Non-Latin letters** (Hindi, Chinese, etc.) technically allowed hain, lekin **English use karna recommended** hai, kyunki code koi bhi padh sakta hai.
- **Reserved words** naam nahi ho sakte (jaise `let`, `class`, `return`, `function`):

```javascript
let let = 5;      // error
let return = 5;   // error
```

### Bina `let` ke assignment (bura tarika)
Bina `use strict` ke `num = 5;` likhne se bhi variable ban jata hai (purane code ki wajah se). Ye **bad practice** hai. **Strict mode me error** aata hai:

```javascript
"use strict";
num = 5; // error: num is not defined
```

## 4. Constants (`const`)
Jis variable ki value **kabhi change nahi honi**, use `const` se declare karo. Dobara value assign karne par **error** aata hai.

```javascript
const myBirthday = '18.04.1982';
myBirthday = '01.01.2001'; // error, reassign nahi kar sakte
```

### Uppercase constants
Jo values **run hone se pehle hi pata hain** aur yaad rakhna mushkil hai (jaise color codes), unke liye **CAPITAL letters aur underscore** wale naam use karte hain.

```javascript
const COLOR_RED = "#F00";
const COLOR_ORANGE = "#FF7F00";

let color = COLOR_ORANGE;
alert(color); // #FF7F00
```

**Fayde:**
- `COLOR_ORANGE` yaad rakhna `"#FF7F00"` se aasan hai.
- Spelling galti kam hoti hai.
- Code padhne me matlab clear rehta hai.

### Kab CAPITAL, kab normal naam?
- **CAPITAL:** jab value **pehle se hard-coded** ho (jaise `COLOR_RED`).
- **Normal naam:** jab value **run-time par calculate** ho, chahe wo baad me change na ho. Jaise `const pageLoadTime = ...;`

## 5. Naam sahi rakho (bahut important)
Variable naam ka **saaf matlab** hona chahiye. Naming programming ki sabse important skills me se ek hai. Naam dekhkar pata chal jata hai ki code beginner ne likha hai ya experienced ne.

**Acche rules:**
- **Human-readable** naam: `userName`, `shoppingCart`
- **Chhote abbreviations** (`a`, `b`, `c`) se bacho
- **Descriptive** naam rakho. `data` aur `value` jaise naam kuch nahi batate.
- **Team me terms fix rakho.** Visitor ko "user" kehte ho to `currentUser`, `newUser` likho (`currentVisitor` nahi).

### Reuse ya naya banayein?
Kuch log naya variable banane ki jagah **purana reuse** karte hain. Isse variable ek aisa dabba ban jata hai jisme **kuch bhi** pada hota hai aur pata nahi andar kya hai. Thoda time bachta hai lekin **debugging me 10 guna zyada** lagta hai.

**Extra variable achha hota hai, bura nahi.** Modern browsers code optimize kar lete hain, performance ki problem nahi hoti.

## Tasks (Practice)

**1. Variables ke saath kaam:**
```javascript
let admin, name;
name = "John";
admin = name;
alert( admin ); // "John"
```

**2. Sahi naam dena:**
```javascript
let ourPlanetName = "Earth";
let currentUserName = "John";
```

**3. Uppercase const?**
- `birthday` → **UPPERCASE ho sakta hai** (value hard-coded hai)
- `age` → **lowercase rahega** (run-time par calculate hoti hai, saal ke saath badalti hai)

## Quick Summary

| Keyword | Matlab |
|---------|--------|
| `let` | Modern variable, value badal sakte hain |
| `const` | Value change nahi ho sakti |
| `var` | Purana tarika, abhi use nahi karte |

| Topic | Yaad rakhne wali baat |
|-------|----------------------|
| Naming | Letters, digits, `$`, `_` allowed. Digit se shuru nahi |
| Style | camelCase use karo |
| Declare | Sirf ek baar |
| Naam | Saaf aur descriptive rakho |
| Constants | Hard-coded values ke liye `UPPER_CASE` |
