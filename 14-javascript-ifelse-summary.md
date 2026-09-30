# Conditional Branching: if, '?' – Simple Hinglish Summary

Source: https://javascript.info/ifelse

## Main baat
Kabhi kabhi **alag conditions ke hisaab se alag kaam** karna padta hai. Iske liye `if` statement aur conditional operator `?` (question mark / ternary) use hote hain.

## 1. `if` statement
`if(...)` bracket ke andar ki **condition check** karta hai. Agar result `true` ho to code block chalta hai.

```javascript
let year = prompt('In which year was ECMAScript-2015 specification published?', '');

if (year == 2015) alert( 'You are right!' );
```

**Ek se zyada statements** chalane ho to **curly braces `{}`** lagane padte hain:

```javascript
if (year == 2015) {
  alert( "That's correct!" );
  alert( "You're so smart!" );
}
```

**Recommendation:** Ek hi statement ho tab bhi **hamesha `{}` lagao.** Readability badhti hai.

## 2. Boolean conversion
`if (...)` condition ko **boolean me convert** karta hai.

- **Falsy values** (`false` ban jati hain): `0`, `""`, `null`, `undefined`, `NaN`
- **Truthy values** (`true` ban jati hain): baaki sab

```javascript
if (0) {
  // kabhi nahi chalega (0 falsy hai)
}

if (1) {
  // hamesha chalega (1 truthy hai)
}
```

Pehle se calculate ki hui boolean value bhi de sakte ho:

```javascript
let cond = (year == 2015);

if (cond) {
  ...
}
```

## 3. `else` clause
Condition **falsy** ho to `else` block chalta hai. (Optional hai.)

```javascript
if (year == 2015) {
  alert( 'You guessed it right!' );
} else {
  alert( 'How can you be so wrong?' );
}
```

## 4. `else if` (kai conditions)
Kai variants check karne ho to `else if` use karo:

```javascript
if (year < 2015) {
  alert( 'Too early...' );
} else if (year > 2015) {
  alert( 'Too late' );
} else {
  alert( 'Exactly!' );
}
```

Pehle `year < 2015` check hota hai. Falsy ho to agli condition, aur sab falsy ho to aakhri `else`. Jitne chahe `else if` laga sakte ho. Aakhri `else` optional hai.

## 5. Conditional operator `?` (ternary)
Kabhi kabhi condition ke hisaab se **variable me value assign** karni hoti hai.

**`if..else` se:**
```javascript
let accessAllowed;
let age = prompt('How old are you?', '');

if (age > 18) {
  accessAllowed = true;
} else {
  accessAllowed = false;
}
```

**`?` se (chhota tarika):**
```javascript
let result = condition ? value1 : value2;
```

Condition truthy ho to `value1`, warna `value2` milta hai.

```javascript
let accessAllowed = (age > 18) ? true : false;
```

- Ise **"ternary"** bhi kehte hain kyunki iske **3 operands** hote hain. JS me ye akela aisa operator hai.
- `age > 18` ke around brackets **zaruri nahi**, lekin **readability ke liye recommended** hain.

**Note:** Upar wale example me `?` ki zarurat hi nahi, kyunki comparison khud `true/false` deta hai:

```javascript
let accessAllowed = age > 18;
```

## 6. Multiple `?`
Kai `?` ek ke baad ek laga kar **kai conditions** handle kar sakte ho:

```javascript
let age = prompt('age?', 18);

let message = (age < 3)   ? 'Hi, baby!' :
              (age < 18)  ? 'Hello!' :
              (age < 100) ? 'Greetings!' :
              'What an unusual age!';

alert( message );
```

**Kaise kaam karta hai:**
1. Pehle `age < 3` check hota hai. True ho to `'Hi, baby!'`.
2. Nahi to `:` ke baad wali condition `age < 18`, true ho to `'Hello!'`.
3. Nahi to `age < 100`, true ho to `'Greetings!'`.
4. Nahi to aakhri value `'What an unusual age!'`.

Ye `if..else if..else` jaisa hi hai.

## 7. `?` ka galat use (avoid karo)
Kuch log `?` ko `if` ki jagah use karte hain:

```javascript
(company == 'Netscape') ?
   alert('Right!') : alert('Wrong.');
```

**Ye recommend nahi hai.** Chhota zaroor hai, lekin **kam readable** hai. Aankhein code ko **vertically scan** karti hain, isliye multi-line blocks samajhna aasan hota hai.

**Behtar tarika:**
```javascript
if (company == 'Netscape') {
  alert('Right!');
} else {
  alert('Wrong.');
}
```

**Rule:** `?` ka kaam **ek value ya dusri value return karna** hai, uska use usi ke liye karo. **Alag code branches chalane ho to `if` use karo.**

## Practice Tasks (Answers)

**1. `if ("0")` me alert dikhega?**
**Haan.** `"0"` khali string nahi hai, isliye truthy hai.

```javascript
if ("0") {
  alert( 'Hello' );   // chalega
}
```

**2. JavaScript ka official naam:**
```javascript
let value = prompt('What is the "official" name of JavaScript?', '');

if (value == 'ECMAScript') {
  alert('Right!');
} else {
  alert("You don't know? ECMAScript!");
}
```

**3. Sign dikhao (1, -1 ya 0):**
```javascript
let value = prompt('Type a number', 0);

if (value > 0) {
  alert( 1 );
} else if (value < 0) {
  alert( -1 );
} else {
  alert( 0 );
}
```

**4. `if` ko `?` me badlo:**
```javascript
let result = (a + b < 4) ? 'Below' : 'Over';
```

**5. `if..else` ko multiple `?` me badlo:**
```javascript
let message = (login == 'Employee') ? 'Hello' :
  (login == 'Director') ? 'Greetings' :
  (login == '') ? 'No login' :
  '';
```

## Quick Summary
| Construct | Kab use karein |
|-----------|----------------|
| `if` | Condition true ho to code chalana |
| `else` | Condition false ho to alag code chalana |
| `else if` | Kai conditions check karni ho |
| `? :` | Condition ke hisaab se **value assign** karni ho |

**Yaad rakho:**
- `{}` hamesha lagao.
- `0`, `""`, `null`, `undefined`, `NaN` falsy hain, baaki truthy (`"0"` bhi truthy hai).
- `?` sirf value return karne ke liye, code branches ke liye `if` use karo.
