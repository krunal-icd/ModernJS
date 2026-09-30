# 25. Coordinates

Elements ko move/position karne ke liye coordinates samajhna zaruri hai. Do systems hain:

| System | Naam | Kaisa |
|---|---|---|
| Window ke hisaab se | `clientX / clientY` | `position:fixed` jaisa, window ke top/left se |
| Document ke hisaab se | `pageX / pageY` | `position:absolute` (document root) jaisa, document ke top/left se |

Page scroll karne par window-relative coordinates **badal jaate hain**, document-relative wahi rehte hain.

Formula: `pageY = clientY + scroll hua vertical hissa`

## `elem.getBoundingClientRect()`
Element ko ghere hue rectangle ke **window coordinates** deta hai (`DOMRect`).
- Main: `x`, `y`, `width`, `height`
- Derived: `left` (=x), `top` (=y), `right` (=x + width), `bottom` (=y + height)

Dhyan:
- Values decimal ho sakti hain (`10.5`), negative bhi (element window ke upar nikal gaya ho).
- `right/bottom` yaha **top-left corner se** count hote hain, CSS ki tarah edge se nahi.
- IE `x/y` support nahi karta, to `top/left` use karo.

## `document.elementFromPoint(x, y)`
Window coordinates `(x, y)` par sabse **nested** element deta hai.
```js
let elem = document.elementFromPoint(centerX, centerY);
```
Coordinates window ke bahar ho (negative ya width/height se zyada) to `null` deta hai, isliye check karo warna error aayega.

## Element ke paas kuch dikhana (fixed)
```js
let coords = elem.getBoundingClientRect();
message.style.cssText = "position:fixed; color: red";
message.style.left = coords.left + "px";
message.style.top = coords.bottom + "px";   // px mat bhulo!
```
Problem: `position:fixed` hone se scroll karne par message element se door bhaag jaata hai.

## Document coordinates (absolute)
Koi standard method nahi hai, khud bana lo:
```js
function getCoords(elem) {
  let box = elem.getBoundingClientRect();
  return {
    top: box.top + window.pageYOffset,
    right: box.right + window.pageXOffset,
    bottom: box.bottom + window.pageYOffset,
    left: box.left + window.pageXOffset
  };
}
```
Isse `position:absolute` ke saath use karo to scroll par message element ke paas hi rahega.

## Summary
- **Window coordinates** (`getBoundingClientRect`) -> `position:fixed` ke saath achhe.
- **Document coordinates** (`getBoundingClientRect` + scroll) -> `position:absolute` ke saath achhe.

## Tasks (short)
- Outer corners: `[coords.left, coords.top]` aur `[coords.right, coords.bottom]`.
- Inner upper-left: `coords.left + field.clientLeft`, `coords.top + field.clientTop`.
- Inner bottom-right: `coords.left + clientLeft + clientWidth`, `coords.top + clientTop + clientHeight`.
- Note ko anchor ke paas dikhana (`positionAt`): anchor ke `getBoundingClientRect()` se `top/right/bottom` ke hisaab se coordinates nikaalo. Absolute version ke liye `getCoords()` use karo.
