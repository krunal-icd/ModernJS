# 42. Scripts: async aur defer

## Problem
Browser jab HTML me `<script>` dekhta hai to DOM banana **rok** deta hai aur script download + execute karta hai. Isse do dikkatein:
1. Script apne **neeche ke DOM elements nahi dekh sakti**.
2. Upar bhaari script ho to **page block** ho jaata hai (user ko content nahi dikhta).

Purana workaround: script `<body>` ke bilkul neeche rakho. Par ye perfect nahi (browser script ko HTML poora download hone ke baad hi dekhta hai, lambe HTML me deri).

Solution: **`defer`** aur **`async`** attributes.

## `defer`
- Script background me download hoti hai, HTML/DOM banna **nahi rukta**.
- DOM **poora ban jaane par** chalti hai (par `DOMContentLoaded` se pehle).
- **Order safe rehta hai** (document me jaisa likha, waisa hi execute), chahe chhoti script pehle download ho jaye.
- `DOMContentLoaded` deferred scripts ka wait karta hai.
- Sirf **external** scripts ke liye (`src` ke bina ignore).
```html
<script defer src="long.js"></script>
<script defer src="small.js"></script>   <!-- long.js ke baad hi chalegi -->
```

## `async`
- Script **poori tarah independent**: koi kisi ka wait nahi karta.
- DOM/dusri scripts async ka wait nahi karti, aur async unka nahi.
- `DOMContentLoaded` async ke pehle ya baad kabhi bhi aa sakta hai (guarantee nahi).
- **Load-first order**: jo pehle download hui wo pehle chalegi.
- Kaam ki jagah: counters, ads, analytics jaise independent third-party scripts.
```html
<script async src="https://google-analytics.com/analytics.js"></script>
```
- Ye bhi sirf external scripts ke liye.

## Dynamic scripts
```js
let script = document.createElement('script');
script.src = "/long.js";
document.body.append(script);   // append hote hi load shuru
```
- Ye **default me `async`** jaisi behave karti hain (load-first).
- Order chahiye to `script.async = false` set karo (tab `defer` jaisa document order):
```js
function loadScript(src) {
  let script = document.createElement('script');
  script.src = src;
  script.async = false;
  document.body.append(script);
}
```

## Farak (summary table)
| | Order | `DOMContentLoaded` |
|---|---|---|
| `async` | Load-first, document order matter nahi | Irrelevant, pehle ya baad kabhi |
| `defer` | Document order | Document parse hone ke baad, `DOMContentLoaded` se just pehle |

**Kab kya:** `defer` = jab poora DOM chahiye ya scripts ka order matter kare (library + uspar depend script). `async` = independent scripts.

## Dhyan rakho
`defer/async` se user page **script load hone se pehle** dekh leta hai. Isliye "loading" indicator dikhao aur jo buttons abhi kaam ke nahi unhe disable rakho.
