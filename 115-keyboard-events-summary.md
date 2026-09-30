# 35. Keyboard: keydown aur keyup

> Input field ke kisi bhi change ko pakadna ho (paste, speech, mouse) to keyboard events kaafi nahi hain, `input` event use karo (agla topic). Keyboard events tab use karo jab **keyboard ki actions** handle karni hon (arrow keys, hotkeys).

## Events
- `keydown`: key dabai (lambi der dabao to **auto-repeat**, baar baar `keydown`)
- `keyup`: key chhodi

Auto-repeat wale events me `event.repeat === true`.

## `event.key` vs `event.code`
- **`event.key`**: character (jaise `z` ya `Z`), language/case ke saath badalta hai.
- **`event.code`**: **physical key** ka code, hamesha same.

| Key | `key` | `code` |
|---|---|---|
| `Z` | `z` | `KeyZ` |
| `Shift+Z` | `Z` | `KeyZ` |
| `F1` | `F1` | `F1` |
| `Shift` | `Shift` | `ShiftLeft` / `ShiftRight` |

Codes: letters `KeyA`..., digits `Digit0`..., special: `Enter`, `Backspace`, `Tab`.
Case matters: `"KeyZ"` sahi, `"keyZ"` galat.

### Kaun sa use karein?
- **Hotkey jo language switch par bhi chale** -> `event.code`
```js
document.addEventListener('keydown', function(event) {
  if (event.code == 'KeyZ' && (event.ctrlKey || event.metaKey)) alert('Undo!');
});
```
- **Layout-dependent character** chahiye -> `event.key`

Dhyan: `event.code` alag layouts me alag character de sakta hai. German layout (QWERTZ) me `Y` dabane par `code = KeyZ` aata hai. (Ye kuch hi codes ke saath hota hai: `KeyA`, `KeyQ`, `KeyZ` jaise.)

## Default actions
`keydown` par kai cheezein hoti hain: character aana, delete, page scroll (`PageDown`), `Ctrl+S` save dialog. `keydown` me `preventDefault()` se zyaadatar roke ja sakte hain. (OS-level keys jaise Windows me `Alt+F4` nahi rok sakte.)

Example, phone input me sirf digits aur `+ ( ) -`:
```js
function checkPhoneKey(key) {
  return (key >= '0' && key <= '9') ||
    ['+','(',')','-','ArrowLeft','ArrowRight','Delete','Backspace'].includes(key);
}
// <input onkeydown="return checkPhoneKey(event.key)" type="tel">
```
`return false` (DOM property / attribute se) default action rokta hai. Filter me `Backspace`, arrows jaise special keys bhi allow karne padte hain, warna wo bhi kaam nahi karenge.

Ye filter 100% bharosemand nahi: mouse se paste ya mobile ke dusre input tarike bach nikalte hain. Behtar: `oninput` se **baad me** value check karo (ya dono use karo).

## Legacy (mat use karo)
`keypress` event aur `keyCode`, `charCode`, `which` properties purani hain, browsers me inconsistent thin, ab deprecated.

## Mobile keyboards (IME)
Virtual keyboards par standard ke hisaab se `keyCode = 229` aur `key = "Unidentified"` ho sakta hai. Kuch keys (arrows, backspace) par sahi values aati hain, par guarantee nahi, to mobile par keyboard logic bharosemand nahi.

## Summary
- Har key (Shift, Ctrl bhi) keyboard event deti hai. Sirf `Fn` key ka event nahi aata.
- `keydown` (auto-repeat), `keyup`.
- `code` = physical key, `key` = character.
- Form fields ke input ke liye `input`/`change` events use karo, keyboard events tab jab keyboard hi chahiye (hotkeys, special keys).

## Task
**Extended hotkeys** (`runOnKeys(func, "KeyQ", "KeyW")`): ek `Set` me abhi dabi hui keys rakho. `keydown` par add, `keyup` par remove. Har `keydown` par check karo ki saari zaruri keys set me hain to `func` chalao.
