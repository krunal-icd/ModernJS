# 06. Export aur Import

## Export ke tarike
**1. Declaration ke aage:**
```js
export let months = ['Jan', 'Feb'];
export const YEAR = 2015;
export class User { constructor(name) { this.name = name; } }
export function sayHi() {}   // function/class ke baad ; nahi lagate
```

**2. Alag se list bana ke:**
```js
function sayHi() {}
function sayBye() {}
export { sayHi, sayBye };
```

## Import ke tarike
```js
import { sayHi, sayBye } from './say.js';   // named
import * as say from './say.js';            // sab kuch ek object me: say.sayHi()
```
Explicit list behtar hai (chhote naam, code ka overview saaf). Bundlers unused cheezein hata dete hain, to zyada import karne se darne ki zarurat nahi.

## `as` se rename
```js
import { sayHi as hi } from './say.js';
export { sayHi as hi, sayBye as bye };
```

## Export Default
Jab module me **sirf ek main cheez** ho:
```js
// user.js
export default class User { ... }

// main.js
import User from './user.js';   // curly braces nahi
```
- Ek file me sirf **ek** `export default`.
- Default export ka naam nahi bhi de sakte.
- Import karte waqt koi bhi naam de sakte ho (isliye kuch teams sirf named exports prefer karti hain).

| Named | Default |
|---|---|
| `export class User {}` | `export default class User {}` |
| `import { User } from ...` | `import User from ...` |

Dono saath: `import { default as User, sayHi } from './user.js'`.

## Re-export
Import + export ek saath. Package ka ek "main" entry file banane me kaam aata hai:
```js
// auth/index.js
export { login, logout } from './helpers.js';
export { default as User } from './user.js';
```
- Re-exported cheezein us file me khud **use nahi** kar sakte.
- `export * from './user.js'` **default ko re-export nahi karta**. Uske liye alag likhna padta hai:
  ```js
  export * from './user.js';
  export { default } from './user.js';
  ```

## Summary
- Import: `import {x}`, `import x`, `import * as obj`, `import "module"` (sirf code run karne ke liye).
- `import/export` **top-level par hi** likh sakte ho, `if` ya `{}` block me nahi.
- Conditional import chahiye? Agla topic: **dynamic import**.
