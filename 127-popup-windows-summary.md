# 47. Popups aur Window Methods

Popup window purana tarika hai dusra document dikhane ka (bina main window band kiye):
```js
window.open('https://javascript.info/')
```
Aajkal zyaadatar browsers naye **tab** me kholte hain. Ab zyaadatar `fetch` + `<div>` jaise dusre tarike use hote hain, mobile par popups tricky hain. Par kuch cheezein ab bhi popups se hoti hain, jaise **OAuth login** (Google/Facebook), kyunki:
1. Popup ka **alag independent JS environment** hota hai (untrusted third-party site kholna safe).
2. Kholna aasan.
3. Popup navigate kar sakta hai aur **opener window ko messages** bhej sakta hai.

## Popup blocking
Purane zamane me evil sites bahut popups kholti thi. Ab browsers **user ke action (jaise `onclick`) ke bahar** ke `window.open` ko block karte hain.
```js
window.open('https://javascript.info');   // blocked

button.onclick = () => {
  window.open('https://javascript.info'); // allowed
};
```

## `window.open(url, name, params)`
- `url`: naye window me load hone wala URL
- `name`: window ka naam. Is naam ki window pehle se hai to usi me khulega, nahi to nayi.
- `params`: settings, comma se alag, **spaces nahi** (jaise `width=200,height=100`).

Settings:
- Position: `left/top` (screen ke bahar nahi jaa sakti), `width/height` (minimum limit hai, invisible window nahi ban sakti)
- Features: `menubar`, `toolbar`, `location`, `status`, `resizable`, `scrollbars` (yes/no). Resizable aur scrollbars band karna recommended nahi.

Rules:
- 3rd argument na ho ya khaali ho: default parameters.
- params diye par kuch `yes/no` features chhod diye: wo **`no`** maane jaate hain. Jo chahiye unhe explicitly `yes` do.
- `left/top` nahi diya: pichhli kholi window ke paas.
- `width/height` nahi diya: pichhli window ke barabar.

Bahut se browsers "ajeeb" values (zero size, offscreen) ko theek kar dete hain.

## Popup ko window se access karna
`open` naye window ka **reference** deta hai:
```js
let newWin = window.open("about:blank", "hello", "width=200,height=200");
newWin.document.write("Hello, world!");
```
`open` ke turant baad naya window **abhi load nahi** hua hota (URL `about:blank`), isliye badalne ke liye `onload` (ya `DOMContentLoaded`) ka wait karo:
```js
newWindow.onload = function() {
  newWindow.document.body.insertAdjacentHTML('afterbegin', html);
};
```
**Same Origin policy:** windows ek doosre ke content ko tabhi freely access kar sakti hain jab **same origin** (protocol://domain:port) ho.

## Window ko popup se access karna
Popup me `window.opener` = opener window ka reference. (Popups ke alawa baaki windows me `null`.)
```js
newWin.document.write("<script>window.opener.document.body.innerHTML = 'Test'<\/script>");
```
Connection **dono taraf** ka hai.

## Popup band karna
- `win.close()`
- `win.closed`: band hai to `true`

`close()` sirf `window.open()` se bani windows par kaam karta hai (baaki ke liye ignore). User kabhi bhi band kar sakta hai, isliye `closed` check karna chahiye.

## Move aur resize
- `win.moveBy(x, y)`, `win.moveTo(x, y)`
- `win.resizeBy(width, height)`, `win.resizeTo(width, height)`
- `window.onresize` event

Misuse rokne ke liye browsers inhe zyaadatar sirf apne khole hue popups par (extra tabs ke bina) chalne dete hain. Window minimize/maximize karne ka JS me tarika nahi hai, aur inke maximized/minimized par move/resize nahi chalta.

## Scroll karna
`win.scrollBy(x, y)`, `win.scrollTo(x, y)`, `elem.scrollIntoView(top = true)`, `window.onscroll` event.

## Focus/blur on window
`window.focus()`, `window.blur()` aur `focus/blur` events hain, par **bahut limited** kyunki purane zamane me abuse hote the (jaise `window.onblur = () => window.focus()` se user ko window me "lock" karna).
- Mobile browsers `window.focus()` ignore karte hain, aur popup naye tab me khule to focusing nahi chalti.
- Useful cases: popup kholne par `newWindow.focus()`; app kab use ho rahi hai ye track karne ke liye `window.onfocus/onblur` (animations ko pause/resume). Par `blur` ka matlab window background me hai, dikh abhi bhi sakti hai.

## Summary
- `open(url, name, params)` popup kholta hai aur reference deta hai. User action ke bahar ke calls block hote hain.
- Sizes diye to popup window, nahi to naya tab.
- Popup se opener: `window.opener`.
- Same origin ho to dono freely ek doosre ko padh/badal sakte hain. Warna sirf `location` badal sakte hain aur **messages** bhej sakte hain (agla topic).
- Popup kholne ka icon/indication dikhana achhi practice hai.
