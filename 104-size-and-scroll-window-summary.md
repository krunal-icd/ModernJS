# 24. Window ki Size aur Scrolling

Root element `document.documentElement` (`<html>`) hai.

## Window ki width/height
```js
document.documentElement.clientWidth
document.documentElement.clientHeight
```
Ye **scrollbar ko hata ke** available (visible) area deta hai. Ye hi usually chahiye hota hai.

`window.innerWidth/innerHeight` **scrollbar ko include** karte hain, to dono alag values de sakte hain.

Hamesha `<!DOCTYPE HTML>` likho, warna geometry properties ajeeb behave kar sakti hain.

## Poore document ki height (scroll hua hissa bhi)
`documentElement.scrollHeight` akela bharosemand nahi (kuch browsers me `clientHeight` se bhi kam aa sakta hai). Isliye **maximum** lo:
```js
let scrollHeight = Math.max(
  document.body.scrollHeight, document.documentElement.scrollHeight,
  document.body.offsetHeight, document.documentElement.offsetHeight,
  document.body.clientHeight, document.documentElement.clientHeight
);
```
(Ye purane zamane ki inconsistencies ki wajah se hai.)

## Current scroll padhna
```js
window.pageYOffset   // upar se kitna scroll
window.pageXOffset   // left se kitna
```
- Read-only. Ye `window.scrollY` / `window.scrollX` ke alias hain.

## Page ko scroll karna
Page ka DOM pura ban chuka ho tab hi kaam karta hai (`<head>` ki script me nahi chalega).

- `window.scrollBy(x, y)`: **current position se relative** (jaise `scrollBy(0, 10)` = 10px neeche)
- `window.scrollTo(pageX, pageY)`: **absolute position** (jaise `scrollTo(0, 0)` = sabse upar)
- `elem.scrollIntoView(top)`:
  - `top = true` (default): element window ke **upar** align
  - `top = false`: element window ke **neeche** align

## Scrolling band karna
```js
document.body.style.overflow = "hidden";   // scroll freeze
document.body.style.overflow = "";         // wapas
```
Dusre elements par bhi kaam karta hai. Nuksaan: scrollbar gayab hone se content "jump" kar sakta hai. Fix: pehle-baad `clientWidth` compare karke `padding` daal do.

## Summary
| Kaam | Kaise |
|---|---|
| Visible window size | `documentElement.clientWidth/Height` |
| Poore document ki size | upar wala `Math.max(...)` |
| Current scroll | `window.pageYOffset / pageXOffset` |
| Scroll karna | `scrollTo`, `scrollBy`, `scrollIntoView` |
