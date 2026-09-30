# 41. Page Lifecycle: DOMContentLoaded, load, beforeunload, unload

HTML page ke lifecycle me 3 important events hain:

| Event | Kab | Kaam ka |
|---|---|---|
| `DOMContentLoaded` | HTML pura load, DOM tree ban gaya (images/styles abhi baaki ho sakte hain) | DOM nodes dhundhna, interface initialize karna |
| `load` | HTML + saare external resources (images, styles) load | Image sizes, styles applied hone ke baad ka kaam |
| `beforeunload / unload` | User page chhod raha hai | Unsaved changes ka confirm / stats bhejna |

## `DOMContentLoaded`
`document` par hota hai, **sirf `addEventListener`** se lagta hai (`document.onDOMContentLoaded` nahi chalta).
```js
document.addEventListener("DOMContentLoaded", ready);
```
Ye image ka wait **nahi** karta (isliye image ki size 0x0 dikh sakti hai).

### Scripts ke saath
Browser jab `<script>` dekhta hai to pehle use **chalata** hai, tab aage DOM banata hai. To `DOMContentLoaded` un scripts ke **baad** hi aata hai.

Exceptions (jo `DOMContentLoaded` ko block nahi karte):
1. `async` attribute wale scripts
2. `document.createElement('script')` se dynamically bane scripts

### Styles ke saath
External stylesheet DOM ko affect nahi karti, to `DOMContentLoaded` uska wait nahi karta. **Par** agar stylesheet ke baad script hai, to wo script stylesheet ke load hone tak ruka rehta hai (style-dependent cheezein padhne ke liye), aur `DOMContentLoaded` us script ka wait karta hai. To indirectly styles ka bhi wait hota hai.

### Autofill
Firefox, Chrome, Opera `DOMContentLoaded` par form autofill karte hain. Agar bhaari scripts isse late karein, to login/password fields der se bharte hain.

## `window.onload`
Poora page + saare resources load hone par. Yaha image sizes sahi milte hain.
```js
window.onload = function() { ... };   // ya window.addEventListener('load', ...)
```

## `window.onunload`
User page chhodte waqt. Yaha aisa kaam karo jisme **der na lage** (jaise related popup band karna).
Analytics bhejni ho to **`navigator.sendBeacon`**:
```js
window.addEventListener("unload", function() {
  navigator.sendBeacon("/analytics", JSON.stringify(analyticsData));
});
```
- POST request jaati hai, background me (page chhodne me deri nahi)
- Data limit 64kb
- Response nahi mil sakta
- `fetch` me `keepalive` flag bhi isi kaam ke liye hai.

Page chhodne ka transition **cancel** karna ho to `unload` me nahi, `beforeunload` me hota hai.

## `window.onbeforeunload`
User page chhodne ya window band karne ki koshish kare to confirmation maangta hai.
```js
window.onbeforeunload = function() { return false; };
```
- Purane browsers non-empty string return karne par wo message dikhate the, ab **custom message nahi** dikhta (misuse ki wajah se).
- `event.preventDefault()` yaha aksar **kaam nahi karta**. Iski jagah `event.returnValue = "..."` set karo:
```js
window.addEventListener("beforeunload", (event) => {
  event.returnValue = "There are unsaved changes. Leave now?";
});
```

## `readyState`
Agar `DOMContentLoaded` handler document ke load hone ke **baad** lagaya, to wo kabhi nahi chalega. Isliye `document.readyState` check karo:
- `"loading"`: document load ho raha hai
- `"interactive"`: document poora padha ja chuka (`DOMContentLoaded` se just pehle)
- `"complete"`: document + saare resources load (`window.onload` se just pehle)

```js
if (document.readyState == 'loading') {
  document.addEventListener('DOMContentLoaded', work);
} else {
  work();   // DOM pehle se ready
}
```
`readystatechange` event bhi hai (state badalne par), par ab kam use hota hai.

### Typical order
1. `readyState: loading`
2. `readyState: interactive`
3. `DOMContentLoaded`
4. iframe `onload`
5. img `onload`
6. `readyState: complete`
7. `window onload`

## Summary
- `DOMContentLoaded`: DOM ready (scripts block karte hain, images ka wait nahi).
- `load`: sab kuch load (kam use, kyunki der lagti hai).
- `beforeunload`: leave confirm; `unload`: sirf simple kaam, analytics ke liye `sendBeacon`.
- `readyState` se pata karo ki document kis state me hai.
