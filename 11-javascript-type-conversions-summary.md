# Type Conversions – Simple Hinglish Summary

Source: https://javascript.info/type-conversions

## Main baat
Operators aur functions zyadatar values ko **automatically sahi type me convert** kar dete hain. Jaise `alert` har value ko string me badalkar dikhata hai, aur math operations values ko number me badalte hain. Kabhi kabhi hume **khud convert** karna padta hai.

(Is chapter me sirf **primitives** ki baat hai, objects baad me.)

## 1. String Conversion
Jab value ka string form chahiye (jaise `alert` me), tab hota hai. Khud karne ke liye `String(value)`:

```javascript
let value = true;
alert(typeof value); // boolean

value = String(value); // ab "true" (string)
alert(typeof value); // string
```

Zyadatar obvious hai: `false` → `"false"`, `null` → `"null"`.

## 2. Numeric Conversion
Math operations me **automatically** hota hai:

```javascript
alert( "6" / "2" ); // 3
```

Khud karne ke liye `Number(value)`:

```javascript
let str = "123";
let num = Number(str);   // 123 (number)
```

Ye tab zaruri hota hai jab form/text input se value string me aati hai lekin number chahiye.

Agar string valid number nahi hai to result **`NaN`** aata hai:

```javascript
let age = Number("an arbitrary string instead of a number");
alert(age); // NaN
```

### Numeric conversion ke rules

| Value | Kya banti hai |
|-------|---------------|
| `undefined` | `NaN` |
| `null` | `0` |
| `true` / `false` | `1` / `0` |
| `string` | Start aur end ki whitespace hat jati hai. Bacha string khali ho to `0`, warna number "padha" jata hai. Error par `NaN`. |

```javascript
alert( Number("   123   ") ); // 123
alert( Number("123z") );      // NaN
alert( Number(true) );        // 1
alert( Number(false) );       // 0
```

**Dhyan:** `null` → `0` lekin `undefined` → `NaN`. Dono alag behave karte hain.

## 3. Boolean Conversion
Sabse simple. Logical operations me automatically hota hai, khud karne ke liye `Boolean(value)`.

**Rule:**
- **"Khali" values** `false` ban jati hain: `0`, `""` (empty string), `null`, `undefined`, `NaN`
- **Baaki sab** `true`

```javascript
alert( Boolean(1) );       // true
alert( Boolean(0) );       // false
alert( Boolean("hello") ); // true
alert( Boolean("") );      // false
```

**Trap:** `"0"` aur `" "` (sirf space) dono **`true`** hain. JavaScript me koi bhi non-empty string `true` hoti hai (PHP me `"0"` false hota hai, JS me nahi).

```javascript
alert( Boolean("0") ); // true
alert( Boolean(" ") ); // true
```

## Summary

| Conversion | Kab hota hai | Khud kaise karein |
|------------|--------------|-------------------|
| String | Output karte waqt | `String(value)` |
| Number | Math operations me | `Number(value)` |
| Boolean | Logical operations me | `Boolean(value)` |

### Boolean rules
| Value | Result |
|-------|--------|
| `0`, `null`, `undefined`, `NaN`, `""` | `false` |
| Baaki sab | `true` |

### Jahan log galti karte hain
- `undefined` number me **`NaN`** banta hai, `0` nahi.
- `"0"` aur `"   "` boolean me **`true`** hote hain.
