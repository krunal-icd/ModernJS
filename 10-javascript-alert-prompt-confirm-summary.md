# Interaction: alert, prompt, confirm – Simple Hinglish Summary

Source: https://javascript.info/alert-prompt-confirm

## Main baat
Browser me user se interact karne ke liye 3 functions hain: **`alert`**, **`prompt`** aur **`confirm`**.

## 1. `alert`
- Message dikhata hai aur user ke **"OK"** dabane ka wait karta hai.

```javascript
alert("Hello");
```

- Ye mini-window **modal window** kehlati hai. **Modal** ka matlab: jab tak user is window ko band nahi karta (OK nahi dabata), tab tak wo **page ke baaki hisse se interact nahi kar sakta.**

## 2. `prompt`
User se **text input** maangta hai.

```javascript
result = prompt(title, [default]);
```

- **`title`** → user ko dikhane wala text
- **`default`** → input field ki initial value (**optional**, square brackets `[...]` ka matlab optional hota hai)
- Window me hota hai: message, input field, aur **OK / Cancel** buttons.

**Result kya milta hai:**
- User kuch type karke **OK** dabaye → **wo text** milta hai
- User **Cancel** dabaye ya **Esc** dabaye → **`null`** milta hai

```javascript
let age = prompt('How old are you?', 100);

alert(`You are ${age} years old!`); // You are 100 years old!
```

### Internet Explorer ke liye tip
IE me agar `default` nahi doge to input field me `"undefined"` likha aa jata hai. Isliye **hamesha dusra argument do**:

```javascript
let test = prompt("Test", ''); // IE ke liye
```

## 3. `confirm`
User se **yes/no question** poochta hai.

```javascript
result = confirm(question);
```

- Window me **question** aur do buttons: **OK** aur **Cancel.**
- **OK** dabane par → `true`
- **Cancel / Esc** dabane par → `false`

```javascript
let isBoss = confirm("Are you the boss?");

alert( isBoss ); // OK dabaya to true
```

## Summary

| Function | Kaam | Return value |
|----------|------|--------------|
| `alert` | Message dikhata hai | Kuch nahi |
| `prompt` | Text input maangta hai | Text, ya Cancel/Esc par `null` |
| `confirm` | OK/Cancel poochta hai | OK par `true`, Cancel/Esc par `false` |

**Teeno modal hain:** script ka execution **pause** ho jata hai aur user page ke baaki hisse se tab tak interact nahi kar sakta jab tak window band na ho.

### Do limitations
1. Window ki **exact location browser decide karta hai** (aam taur par center me).
2. Window ka **look browser decide karta hai**, hum badal nahi sakte.

Ye simplicity ki keemat hai. Better-looking windows ke liye dusre tarike hain, lekin agar zyada style ki zarurat nahi to ye methods bilkul theek hain.

## Practice Task
**Ek page banao jo naam poochhe aur usse dikhaye:**

```html
<!DOCTYPE html>
<html>
<body>

  <script>
    'use strict';

    let name = prompt("What is your name?", "");
    alert(name);
  </script>

</body>
</html>
```
