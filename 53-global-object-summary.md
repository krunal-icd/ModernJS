# Global Object

## Ye kya hai?
Global object me wo cheezein hoti hain jo **kahin se bhi** accessible hain, jaise built-in functions aur environment ki cheezein.

| Jagah | Naam |
|---|---|
| Browser | `window` |
| Node.js | `global` |
| Har jagah (standard) | `globalThis` |

Agar code alag-alag environments me chalana hai to `globalThis` use karo.

```javascript
alert("Hi");
window.alert("Hi"); // dono same
```

## `var` vs `let` (global level par)
Browser me (modules ke bina):
- Global `var` aur function declaration **window ki property** ban jaate hain.
- Global `let` / `const` **nahi** bante.

```javascript
var a = 5;
let b = 10;

window.a; // 5
window.b; // undefined
```
Ye sirf purani compatibility ke liye hai. Is par depend mat karo. Modern code me modules use hote hain jahan ye hota hi nahi.

## Kuch sach me global chahiye to
Seedha `window` par property bana do:
```javascript
window.currentUser = { name: "John" };
```

## Global variables kam se kam rakho
Function ko input parameters do aur output return karwao. Isse code samajhna aur test karna aasan hota hai.

## Polyfill me use
Global object se check karte hain ki koi modern feature browser me hai ya nahi.
```javascript
if (!window.Promise) {
  // purana browser: yahan apna version bana do (polyfill)
}
```

## Yaad rakho
- Global object = sab jagah available cheezein.
- Universal naam `globalThis`.
- Global values sirf tab rakho jab sach me zaroori ho.
- Clear code ke liye `window.x` likho.
