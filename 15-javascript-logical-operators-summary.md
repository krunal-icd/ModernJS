# Logical Operators – Simple Hinglish Summary

Source: https://javascript.info/logical-operators

## Main baat
JavaScript me **4 logical operators** hain: `||` (OR), `&&` (AND), `!` (NOT), `??` (Nullish Coalescing). Is chapter me pehle 3 hain, `??` agle chapter me hai.

Inhe "logical" kehte hain, lekin ye **kisi bhi type ki value** par lag sakte hain aur result bhi **kisi bhi type ka** ho sakta hai.

## 1. `||` (OR)
```javascript
result = a || b;
```

**Classical boolean logic:** Koi ek bhi `true` ho to `true`, dono `false` ho tabhi `false`.

```javascript
alert( true || true );   // true
alert( false || true );  // true
alert( true || false );  // true
alert( false || false ); // false
```

Non-boolean operand ho to boolean me convert hota hai (`1` → `true`, `0` → `false`).

**Zyadatar `if` me use hota hai:**

```javascript
let hour = 12;
let isWeekend = true;

if (hour < 10 || hour > 18 || isWeekend) {
  alert( 'The office is closed.' );
}
```

## 2. OR pehli truthy value dhundhta hai
JavaScript me `||` **classical se zyada powerful** hai.

```javascript
result = value1 || value2 || value3;
```

**Kaise kaam karta hai:**
1. Operands ko **left se right** evaluate karta hai.
2. Har operand ko boolean me convert karke dekhta hai. `true` mile to **ruk jata hai** aur us operand ki **original value** return karta hai.
3. Sab `false` hon to **aakhri operand** return karta hai.

**Yaani: OR pehli truthy value return karta hai, ya aakhri value agar koi truthy na mile.**

```javascript
alert( 1 || 0 );                  // 1
alert( null || 1 );               // 1
alert( null || 0 || 1 );          // 1
alert( undefined || null || 0 );  // 0 (sab falsy, aakhri wali)
```

### Iske do fayde

**1. List me se pehli truthy value lena:**
```javascript
let firstName = "";
let lastName = "";
let nickName = "SuperCoder";

alert( firstName || lastName || nickName || "Anonymous" ); // SuperCoder
```
Sab falsy hote to `"Anonymous"` dikhta.

**2. Short-circuit evaluation:**
`||` pehli truthy value milte hi **ruk jata hai**, baaki operands ko chhuta bhi nahi. Ye tab important hai jab operand me side effect ho (function call, assignment).

```javascript
true || alert("not printed");   // alert nahi chalega
false || alert("printed");      // alert chalega
```

## 3. `&&` (AND)
```javascript
result = a && b;
```

**Classical logic:** Dono truthy ho tabhi `true`, warna `false`.

```javascript
alert( true && true );   // true
alert( false && true );  // false
alert( true && false );  // false
alert( false && false ); // false
```

```javascript
let hour = 12;
let minute = 30;

if (hour == 12 && minute == 30) {
  alert( 'The time is 12:30' );
}
```

## 4. AND pehli falsy value dhundhta hai
```javascript
result = value1 && value2 && value3;
```

**Kaise kaam karta hai:**
1. Left se right evaluate karta hai.
2. Har operand ko boolean me convert karta hai. `false` mile to **ruk jata hai** aur us operand ki **original value** return karta hai.
3. Sab truthy hon to **aakhri value** return karta hai.

**Yaani: AND pehli falsy value return karta hai, ya aakhri value agar sab truthy hon.**

**OR aur AND ka fark:** OR pehli **truthy** dhundhta hai, AND pehli **falsy** dhundhta hai.

```javascript
alert( 1 && 0 );          // 0
alert( 1 && 5 );          // 5
alert( null && 5 );       // null
alert( 0 && "no matter" ); // 0

alert( 1 && 2 && null && 3 ); // null (pehli falsy)
alert( 1 && 2 && 3 );         // 3 (sab truthy, aakhri)
```

**Precedence:** `&&` ki precedence `||` se **zyada** hai. Yaani `a && b || c && d` = `(a && b) || (c && d)`.

### `if` ki jagah `||` ya `&&` mat use karo
```javascript
let x = 1;

(x > 0) && alert( 'Greater than zero!' );   // chalta hai
if (x > 0) alert( 'Greater than zero!' );   // behtar, zyada clear
```

Dono same kaam karte hain, lekin `if` zyada readable hai. **Jis kaam ke liye jo bana hai, wahi use karo.**

## 5. `!` (NOT)
```javascript
result = !value;
```

1. Operand ko boolean me convert karta hai.
2. **Ulta value** return karta hai.

```javascript
alert( !true ); // false
alert( !0 );    // true
```

### Double NOT `!!`
Value ko **boolean me convert** karne ke liye use hota hai:

```javascript
alert( !!"non-empty string" ); // true
alert( !!null );               // false
```

Pehla `!` convert karke ulta karta hai, dusra `!` wapas ulta kar deta hai. Ye `Boolean(...)` jaisa hi hai:

```javascript
alert( Boolean("non-empty string") ); // true
alert( Boolean(null) );               // false
```

**Precedence:** `!` ki precedence **sabse zyada** hai, ye `&&` aur `||` se pehle chalta hai.

## Practice Tasks (Answers)

**1.** `alert( null || 2 || undefined );` → **`2`** (pehli truthy)

**2.** `alert( alert(1) || 2 || alert(3) );` → pehle **`1`**, phir **`2`**
`alert` `undefined` return karta hai (falsy), isliye OR aage badhta hai. `2` truthy hai, wahin ruk jata hai, `alert(3)` chalta hi nahi.

**3.** `alert( 1 && null && 2 );` → **`null`** (pehli falsy)

**4.** `alert( alert(1) && alert(2) );` → pehle **`1`**, phir **`undefined`**
`alert(1)` `undefined` (falsy) return karta hai, AND wahin ruk jata hai.

**5.** `alert( null || 2 && 3 || 4 );` → **`3`**
`&&` pehle chalta hai: `2 && 3 = 3`. Phir `null || 3 || 4` = `3`.

**6. Age 14 se 90 ke beech (inclusive):**
```javascript
if (age >= 14 && age <= 90)
```

**7. Age 14 se 90 ke beech NAHI hai:**
```javascript
// NOT ke saath
if (!(age >= 14 && age <= 90))

// NOT ke bina
if (age < 14 || age > 90)
```

**8. Kaun se alert chalenge?**
```javascript
if (-1 || 0) alert( 'first' );            // chalega  (-1 truthy)
if (-1 && 0) alert( 'second' );           // nahi     (0 falsy)
if (null || -1 && 1) alert( 'third' );    // chalega  (-1 && 1 = 1)
```
Jawab: **pehla aur teesra.**

**9. Login check (nested if):**
```javascript
let userName = prompt("Who's there?", '');

if (userName === 'Admin') {

  let pass = prompt('Password?', '');

  if (pass === 'TheMaster') {
    alert( 'Welcome!' );
  } else if (pass === '' || pass === null) {
    alert( 'Canceled' );
  } else {
    alert( 'Wrong password' );
  }

} else if (userName === '' || userName === null) {
  alert( 'Canceled' );
} else {
  alert( "I don't know you" );
}
```
(Khali input `''` deta hai aur `Esc` `null` deta hai.)

## Quick Summary

| Operator | Kya karta hai | Return karta hai |
|----------|---------------|------------------|
| `\|\|` (OR) | Pehli truthy dhundhta hai | Pehli truthy, ya aakhri value |
| `&&` (AND) | Pehli falsy dhundhta hai | Pehli falsy, ya aakhri value |
| `!` (NOT) | Value ulti karta hai | `true` / `false` |
| `!!` | Boolean me convert karta hai | `true` / `false` |

**Precedence:** `!` > `&&` > `||`

**Yaad rakho:**
- `||` aur `&&` **original value** return karte hain, sirf `true/false` nahi.
- Dono **short-circuit** karte hain (pehla result mil gaya to baaki chhod dete hain).
- `if` ki jagah `||` / `&&` mat use karo.
