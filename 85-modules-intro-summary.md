# 05. Modules: Introduction

## Module kya hai?
**Ek file = ek module.** Badi app ko chhoti files me todne ke liye. Files ek dusre se `export` aur `import` se baat karti hain.

- `export`: bahar dikhana hai to
- `import`: dusre module se lena hai to

```js
// sayHi.js
export function sayHi(user) { alert(`Hello, ${user}!`); }

// main.js
import { sayHi } from './sayHi.js';
sayHi('John');
```

## Browser me use
```html
<script type="module">
  import { sayHi } from './say.js';
</script>
```
Modules **file:// se nahi chalte**. Local server chahiye (jaise VS Code Live Server).

(Purane systems: AMD, CommonJS, UMD. Ab standard ES Modules use hote hain.)

## Core Features
1. **Hamesha `use strict`** mode me.
2. **Apna alag scope**: ek module ke variables dusre ko nahi dikhte, share karna ho to export/import karo.
3. **Code sirf ek baar chalta hai** (pehle import par). Baad ke importers ko wahi exports milte hain, isliye same object sab share karte hain.
   ```js
   // admin.js
   export let admin = { name: "John" };
   // 1.js: admin.name = "Pete";
   // 2.js: admin.name -> "Pete" (same object)
   ```
   Isse module ko **configure** karna aasan hota hai (config object export karo, pehle script me set karo).
4. **`import.meta`**: current module ki info (browser me `import.meta.url`).
5. Top-level **`this` undefined** hota hai.

## Browser-specific baatein
- Module scripts **deferred** hoti hain (HTML ke baad chalti hain, order bana rehta hai).
- `async` attribute inline module par bhi kaam karta hai.
- Same `src` wali external module script sirf ek baar chalti hai.
- Dusre origin se load karne par **CORS** header chahiye.
- **Bare modules** (`import 'sayHi'` bina path) browser me allowed nahi. Path dena padta hai (`./sayHi.js`).
- Purane browsers ke liye fallback: `<script nomodule>`.

## Build Tools
Real projects me Webpack jaise bundlers modules ko ek file me jodte hain, unused code hatate hain (tree-shaking), minify karte hain, aur modern syntax ko purane me convert karte hain.

## Yaad rakho
Module = file, apna scope, strict mode, ek hi baar execute, import/export se sharing.
