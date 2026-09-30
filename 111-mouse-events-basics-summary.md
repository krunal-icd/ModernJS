# 31. Mouse Events (Basics)

> Ye events sirf mouse se nahi, phones/tablets par bhi (compatibility ke liye) emulate hote hain.

## Mouse event types
- `mousedown / mouseup`: button dabaya / chhoda
- `mouseover / mouseout`: pointer element par aaya / gaya
- `mousemove`: har move par
- `click`: `mousedown` + `mouseup` **same element par** (sirf left button)
- `dblclick`: do baar jaldi click (ab kam use)
- `contextmenu`: right click (keyboard ke special key se bhi aa sakta hai)

## Events ka order (fixed)
Left click: `mousedown` -> `mouseup` -> `click`

## Mouse button: `event.button`
`mousedown/mouseup` me kaam aata hai (ye kisi bhi button par chalte hain).

| Button | `event.button` |
|---|---|
| Left | 0 |
| Middle | 1 |
| Right | 2 |
| X1 (back) | 3 |
| X2 (forward) | 4 |

- `event.buttons` (ek saath dabe hue buttons) rarely use hota hai.
- `event.which` **purana/deprecated** hai, use mat karo.

## Modifier keys
Har mouse event me ye properties hoti hain (pressed ho to `true`):
- `shiftKey`, `altKey` (Mac par Opt), `ctrlKey`, `metaKey` (Mac par Cmd)
```js
button.onclick = function(event) {
  if (event.altKey && event.shiftKey) alert('Hooray!');
};
```
**Mac ka dhyan:** Windows/Linux me `Ctrl` jo kaam karta hai, Mac me `Cmd`. Mac par `Ctrl`+click actually **right click** (`contextmenu`) ban jaata hai. Isliye dono check karo:
```js
if (event.ctrlKey || event.metaKey) { ... }
```
Mobile par keyboard nahi hota, isliye modifier keys sirf **extra** feature ke roop me rakho.

## Coordinates
- `clientX / clientY`: window ke hisaab se (`position:fixed` jaisa)
- `pageX / pageY`: document ke hisaab se (scroll se nahi badalte)

## Selection rokna
Double click ya mouse dabakar ghumane se text **select** ho jaata hai (kabhi kabhi unwanted). Rokne ke liye `mousedown` par default action roko:
```html
<b ondblclick="alert('Click!')" onmousedown="return false">Double-click me</b>
```
(Text ke andar se select shuru nahi hoga, par pehle/baad se ho sakta hai.)

Copy rokna ho to `oncopy` par `return false`. (Par user page source se phir bhi le sakta hai.)

## Summary
- Button: `button`
- Modifiers: `altKey`, `ctrlKey`, `shiftKey`, `metaKey` (Mac ke liye `metaKey || ctrlKey`)
- Coordinates: `clientX/Y` (window), `pageX/Y` (document)
- `mousedown` ka default action = text selection, zarurat ho to roko.

## Task
**Selectable list:** click par sirf wahi `.selected`; `Ctrl`/`Cmd` ke saath click par sirf us item ko toggle karo. Native text selection `mousedown` par roko.
