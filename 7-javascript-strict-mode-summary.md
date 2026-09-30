# The Modern Mode, "use strict" – Simple Hinglish Summary

Source: https://javascript.info/strict-mode

## Main baat
Pehle JavaScript me naye features add hote the lekin purani cheezein **kabhi change nahi hoti thi**, taaki purana code na toote. Iska nuksan ye tha ki JS ke creators ki **galtiyan bhi hamesha ke liye language me reh gayi.**

**2009 me ECMAScript 5 (ES5)** aaya. Usne naye features add kiye aur kuch purani cheezein badli. Lekin purana code chalta rahe, isliye ye badlav **by default OFF** rakhe gaye. Unhe ON karne ke liye ek special directive chahiye: **`"use strict"`**

## 1. "use strict" kaise use karein
- Ye ek **string jaisa dikhta hai:** `"use strict"` ya `'use strict'`.
- Script ke **sabse upar** likho to poori script **modern way** me chalti hai.

```javascript
"use strict";

// ye code modern way me chalega
...
```

- Ise **function ke start me** bhi likh sakte ho. Tab strict mode sirf **us function me** ON hoga (functions aage seekhenge). Lekin aam taur par poori script ke liye use karte hain.

### Zaruri: Hamesha sabse upar likho
Agar upar koi code hai, to `"use strict"` **ignore ho jata hai.**

```javascript
alert("some code");
// niche wala "use strict" ignore hoga, ye upar hona chahiye

"use strict";

// strict mode ON nahi hua
```

`"use strict"` ke upar sirf **comments** ho sakte hain.

### Cancel nahi kar sakte
`"no use strict"` jaisa koi directive **nahi hai.** Ek baar strict mode ON to **wapas nahi ja sakte.**

## 2. Browser Console me use strict
- Developer console me code **by default strict mode me nahi chalta.** Kabhi kabhi is wajah se **galat results** aa sakte hain.
- **Tarika 1:** `Shift + Enter` se multi-line likho aur upar `'use strict'` daalo (Chrome aur Firefox me kaam karta hai):

```javascript
'use strict'; <Shift+Enter for a newline>
//  ...your code
<Enter to run>
```

- **Tarika 2 (purane browsers ke liye, bharosemand):** function wrapper me daalo:

```javascript
(function() {
  'use strict';

  // ...your code here...
})()
```

## 3. Kya hume "use strict" use karna chahiye?
- Modern JS ke **classes** aur **modules** me strict mode **automatically ON** hota hai. Unke saath alag se likhne ki zarurat nahi.
- **Abhi ke liye:** `"use strict";` ko apni scripts ke upar likhna achhi aadat hai. Baad me jab sab code classes/modules me hoga, tab chhod sakte ho.
- Strict aur purane mode ke differences kam hain aur **life aasan banate hain.** Aage chapters me dekhenge.
- **Is tutorial ke saare examples strict mode maan kar chalte hain**, jab tak alag se na likha ho.

## Quick Summary
| Topic | Yaad rakhne wali baat |
|-------|----------------------|
| Kya hai | ES5 ke modern features ON karne wala directive |
| Syntax | `"use strict";` |
| Kahan likhein | Script (ya function) ke **sabse upar** |
| Cancel | Nahi kar sakte |
| Console me | `Shift+Enter` se upar `'use strict'` likho |
| Classes/Modules | Strict mode automatic hota hai |
| Abhi kya karein | Scripts ke upar `"use strict";` likhne ki aadat daalo |
