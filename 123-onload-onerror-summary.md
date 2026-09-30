# 43. Resource Loading: onload aur onerror

Scripts, images, iframes jaise external resources ke loading ko track karne ke 2 events:
- `onload`: successful load
- `onerror`: error hua

## Script load karna
```js
let script = document.createElement('script');
script.src = "my.js";
document.head.append(script);
```
Us script ke functions chalane ke liye load hone ka wait karna padta hai.

### `script.onload`
Script load **aur execute** hone ke baad chalta hai.
```js
script.onload = function() {
  alert(_.VERSION);   // script ka variable use kar sakte ho
};
```

### `script.onerror`
Load fail hone par (jaise 404, server down):
```js
script.onerror = function() { alert("Error loading " + this.src); };
```
Dhyan: **HTTP error details nahi milte** (404 hai ya 500), sirf itna ki load fail hua.

**Important:** `onload/onerror` sirf **loading** track karte hain. Script chalte waqt jo errors aaye unke liye nahi. Script load ho gayi to `onload` chalega chahe usme bugs hon. Runtime errors ke liye `window.onerror` use karo.

## Baaki resources
`load` aur `error` har us resource ke liye chalte hain jiska external `src` ho.
```js
let img = document.createElement('img');
img.src = "train.gif";   // img document me daalne se nahi, src milte hi load shuru
img.onload = ...;
img.onerror = ...;
```
- Zyaadatar resources document me add hone par load shuru karte hain, **`<img>` exception hai** (src milte hi).
- **`<iframe>`**: `onload` hamesha chalta hai (success ho ya error), historical wajah se.

## Crossorigin policy
Ek site ki script dusri site ka content nahi padh sakti (origin = protocol + domain + port). Ye rule resources par bhi lagta hai.

Dusre domain ki script me error ho to `window.onerror` ko sirf **"Script error."** milta hai, details (line, stack) chhupi rehti hain. Details chahiye to:

| `crossorigin` attribute | Matlab |
|---|---|
| Koi nahi | Access mana |
| `crossorigin="anonymous"` | Server `Access-Control-Allow-Origin` (`*` ya hamara origin) bheje to allowed, cookies nahi jaati |
| `crossorigin="use-credentials"` | Server ko `Access-Control-Allow-Origin` (hamara origin) + `Access-Control-Allow-Credentials: true` dena hoga, cookies bhi jaati hain |

```html
<script crossorigin="anonymous" src="https://other-site.com/error.js"></script>
```

## Summary
- `load`: success, `error`: fail.
- `<iframe>` ka `load` hamesha trigger hota hai.
- `readystatechange` bhi chalta hai par kam use hota hai.

## Task: `preloadImages(sources, callback)`
Saari images ko load karke, sab ke load **ya error** hone par `callback()` chalao.
Algorithm: har source ke liye `img` banao, `onload/onerror` lagao, dono me counter badhao, counter == sources ki length to `callback()`.
