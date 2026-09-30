# 29. Browser Default Actions

Kai events ke saath browser **apne aap kuch karta hai** (default action):
- Link par click -> us URL par jaana
- Submit button par click -> form submit
- Mouse dabakar ghumana -> text select
- Right click (`contextmenu`) -> browser ka context menu

Agar hum event khud handle kar rahe hain, to hum default action **rokna** chah sakte hain.

## Default action rokne ke 2 tarike
1. **`event.preventDefault()`** (main tarika)
2. Handler `on<event>` se laga ho to **`return false`** bhi wahi karta hai.
```html
<a href="/" onclick="return false">Click here</a>
<a href="/" onclick="event.preventDefault()">here</a>
```
Handler ki return value aam taur par ignore hoti hai, sirf `on<event>` me `return false` exception hai. `addEventListener` me `return false` ka koi asar nahi.

## Example: JS menu
Menu items `<a>` hi rakho (right click, search engines, accessibility ke liye), par click JS se handle karo:
```js
menu.onclick = function(event) {
  if (event.target.nodeName != 'A') return;
  let href = event.target.getAttribute('href');
  alert(href);
  return false;   // URL par jaane se roko
};
```

## Follow-up events
Kuch events ek ke baad ek aate hain. Pehla roka to doosra nahi aata. Jaise `mousedown` roko to `<input>` par **focus** nahi hota (Tab key se ho jaata hai).

## `passive: true` option
```js
elem.addEventListener('touchmove', handler, { passive: true });
```
Browser ko batata hai ki handler `preventDefault()` **nahi** karega. Isse mobile scrolling jhatke ke bina smooth hoti hai (browser handlers ke khatam hone ka wait nahi karta). Firefox/Chrome me `touchstart`/`touchmove` par ye default `true` hai.

## `event.defaultPrevented`
`true` agar default action roka gaya, warna `false`.

Use case: `stopPropagation()` ke bajay ye signal dena ki event "handle ho chuka":
```js
elem.oncontextmenu = function(event) {
  event.preventDefault();
  alert("Button context menu");
};

document.oncontextmenu = function(event) {
  if (event.defaultPrevented) return;   // pehle hi handle ho gaya
  event.preventDefault();
  alert("Document context menu");
};
```
`stopPropagation()` se bahar ka code (jaise analytics) ko right click ki info nahi milti, isliye ye behtar hai.

`stopPropagation()` aur `preventDefault()` **alag cheezein** hain, ek doosre se related nahi.

## Semantic raho, abuse mat karo
Technically kisi bhi element ka behavior badal sakte ho (link ko button jaisa), par HTML elements ka meaning banaye rakho: `<a>` navigation ke liye, `<button>` action ke liye. Isse accessibility better rehti hai aur "new tab me kholo" jaisi browser features kaam karti hain.

## Kuch aur default actions
`mousedown` (selection shuru), checkbox par `click` (check/uncheck), `submit`, `keydown` (character likhna), `contextmenu`.

## Tasks (short)
- **`return false` kyun nahi chala?** `onclick="handler()"` me browser function banata hai jo `handler()` ko call karke uski value **return nahi karta**. Fix: `onclick="return handler()"` ya `handler(event)` me `event.preventDefault()`.
- **Links ke liye confirm:** container par delegation, `link.getAttribute('href')` use karo.
- **Image gallery:** container par click, `<a>` par `preventDefault()`, main image ka `src` = thumbnail ka `href`.
