# Nullish Coalescing Operator `??` – Simple Hinglish Summary

Source: https://javascript.info/nullish-coalescing-operator

## Main baat
`??` (do question marks) ek **naya operator** hai (purane browsers me polyfill chahiye). Ye **pehli "defined" value** chunne ka short tarika hai.

**"Defined" ka matlab:** jo value **na `null` ho, na `undefined`.**

## Kaise kaam karta hai
`a ?? b` ka result:
- `a` **defined** hai to → `a`
- `a` defined nahi hai (`null`/`undefined`) to → `b`

Ye is code ka short form hai:

```javascript
result = (a !== null && a !== undefined) ? a : b;
```

## Sabse common use: Default value dena
```javascript
let user;
alert(user ?? "Anonymous"); // Anonymous (user undefined hai)

let user2 = "John";
alert(user2 ?? "Anonymous"); // John
```

**Kai `??` chain** karke list me se pehli defined value le sakte ho:

```javascript
let firstName = null;
let lastName = null;
let nickName = "Supercoder";

alert(firstName ?? lastName ?? nickName ?? "Anonymous"); // Supercoder
```

## `||` se comparison (sabse important fark)
- **`||`** → pehli **truthy** value return karta hai
- **`??`** → pehli **defined** value (na null, na undefined) return karta hai

`||` `false`, `0`, `""`, `null`, `undefined` sabko **ek jaisa (falsy)** maanta hai. Lekin kabhi kabhi hume default value **sirf tab** chahiye jab value **`null/undefined`** ho, yaani jab value sach me set na ho.

```javascript
let height = 0;

alert(height || 100); // 100  (0 falsy hai, isliye galat default aaya)
alert(height ?? 100); // 0    (0 defined hai, isliye sahi)
```

`0` height kai baar **valid value** hoti hai, use default se replace nahi karna chahiye. Isliye yahan **`??` sahi kaam karta hai.**

## Precedence (kaun pehle chalega)
`??` ki precedence `||` jaisi hi hai (dono **3**). Yaani `+`, `*` jaise operators pehle chalte hain, `=` aur `?` baad me.

Isliye expressions me **brackets lagana zaruri** hai:

```javascript
let height = null;
let width = null;

let area = (height ?? 100) * (width ?? 50);
alert(area); // 5000
```

Brackets ke bina `*` pehle chal jayega aur galat result aayega:

```javascript
let area = height ?? 100 * width ?? 50;
// ye aise chalta hai (galat): height ?? (100 * width) ?? 50
```

## `&&` ya `||` ke saath `??` (forbidden)
Safety ke liye JS **bina brackets ke** `??` ko `&&` ya `||` ke saath use karne nahi deta:

```javascript
let x = 1 && 2 ?? 3; // Syntax error
```

**Brackets lagao to chalega:**

```javascript
let x = (1 && 2) ?? 3;
alert(x); // 2
```

## Summary
- `??` **pehli defined (na null, na undefined) value** chunne ka chhota tarika hai.
- **Default values** dene ke liye use hota hai:
  ```javascript
  height = height ?? 100;   // height null/undefined ho to 100
  ```
- Precedence kam hai, isliye expressions me **brackets** lagao.
- `||` ya `&&` ke saath **bina brackets ke use nahi kar sakte.**

## Quick Comparison

| Operator | Kya check karta hai | `0` ke saath | `""` ke saath |
|----------|--------------------|--------------|---------------|
| `\|\|` | Truthy / falsy | `0` ko skip karta hai | `""` ko skip karta hai |
| `??` | `null` / `undefined` | `0` ko rakhta hai | `""` ko rakhta hai |

**Yaad rakho:** `0` ya `""` valid value ho sakti hai to `||` ki jagah **`??`** use karo.
