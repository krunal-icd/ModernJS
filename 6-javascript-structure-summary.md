# Code Structure – Simple Hinglish Summary

Source: https://javascript.info/structure

## Main baat
Sabse pehle code ke **building blocks** seekhte hain: **Statements, Semicolons aur Comments.**

## 1. Statements
- Statement wo command hai jo **koi action perform karta hai.** Jaise `alert('Hello, world!')`.
- Jitne chahe statements likh sakte ho. Unhe **semicolon (`;`)** se alag karte hain.

```javascript
alert('Hello'); alert('World');
```

- Readability ke liye **har statement alag line me** likhte hain:

```javascript
alert('Hello');
alert('World');
```

## 2. Semicolons
- Zyadatar cases me **line break ho to semicolon likhna zaruri nahi.** JavaScript khud semicolon laga leta hai. Ise **Automatic Semicolon Insertion (ASI)** kehte hain.

```javascript
alert('Hello')
alert('World')
```

- **Lekin "zyadatar" ka matlab "hamesha" nahi hota.**
- Kuch cases me newline ka matlab semicolon nahi hota. Jaise jab line `+` par khatam ho (incomplete expression):

```javascript
alert(3 +
1
+ 2);   // output: 6
```

### Error ka example (semicolon miss hone par)
Ye sahi chalta hai (`Hello`, `1`, `2` dikhata hai):

```javascript
alert("Hello");

[1, 2].forEach(alert);
```

Semicolon hata diya to **error aata hai:**

```javascript
alert("Hello")

[1, 2].forEach(alert);
```

Kyu? JavaScript **square brackets `[...]` se pehle semicolon nahi lagata**, isliye wo isko ek hi statement samajhta hai:

```javascript
alert("Hello")[1, 2].forEach(alert);
```

Aise errors dhundhna **bahut mushkil** hota hai.

### Recommendation
**Semicolon hamesha lagao**, chahe newline ho. Ye community ka common rule hai aur beginners ke liye safe hai.

## 3. Comments
Program bade hote hain, to code ke saath **explanation (kya aur kyu)** likhna zaruri ho jata hai. Comments ko JS engine **ignore** karta hai, execution par koi effect nahi.

### Single-line comment: `//`
```javascript
// Ye poori line comment hai
alert('Hello');

alert('World'); // Ye statement ke baad wala comment hai
```

### Multi-line comment: `/* ... */`
```javascript
/* Do messages ka example.
Ye multiline comment hai.
*/
alert('Hello');
alert('World');
```

### Code ko temporarily band karna
```javascript
/* Commenting out the code
alert('Hello');
*/
alert('World');   // sirf ye chalega
```

### Hotkeys (bahut kaam ke)
| Kaam | Windows/Linux | Mac |
|------|---------------|-----|
| Single-line comment | `Ctrl + /` | `Cmd + /` |
| Multi-line comment | `Ctrl + Shift + /` | `Cmd + Option + /` |

### Nested comments allowed nahi
`/* ... */` ke andar dusra `/* ... */` **nahi likh sakte**, error aayega.

```javascript
/*
  /* nested comment ?!? */
*/
alert( 'World' );   // ERROR
```

### Comments likhne se darna nahi
Comments code ka size badhate hain, lekin problem nahi. Production me **minify tools comments hata dete hain**, isliye koi nuksan nahi.

## Quick Summary
| Topic | Yaad rakhne wali baat |
|-------|----------------------|
| Statement | Ek action wali command, `;` se alag hoti hai |
| Semicolon | Optional lagta hai, lekin **hamesha lagao** (safe) |
| `//` | Single-line comment |
| `/* */` | Multi-line comment (nested nahi chalta) |
| Comments | Engine ignore karta hai, likhne me hichkichao mat |
