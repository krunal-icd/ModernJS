# 23. Element Size aur Scrolling

Ye properties elements ki width, height aur position ke bare me bataati hain. Sab values **pixels me numbers** hoti hain (string nahi).

## Sample element
`width: 300px; height: 200px; border: 25px; padding: 20px; overflow: auto` (scrollbar ke saath). **Margin ka koi property nahi** hota (wo element ka hissa nahi).

Dhyan: kuch browsers scrollbar ki jagah content se lete hain (jaise 16px), isliye content width `300 - 16 = 284px` ho sakti hai.

## Properties (bahar se andar)

| Property | Matlab | Sample value |
|---|---|---|
| `offsetParent` | sabse paas ka positioned ancestor (ya `td/th/table/body`) | |
| `offsetLeft / offsetTop` | `offsetParent` ke upper-left corner se coordinates | |
| `offsetWidth / offsetHeight` | **outer size**, borders ke saath | 390 / 290 |
| `clientLeft / clientTop` | borders ki width (asal me outer corner se inner corner ki doori) | 25 / 25 |
| `clientWidth / clientHeight` | andar ka area: content + padding, **scrollbar aur border nahi** | 324 / 240 |
| `scrollWidth / scrollHeight` | client jaisa, par **chhupa (scroll hua) hissa bhi** | 324 / 723 |
| `scrollLeft / scrollTop` | kitna hissa **scroll ho chuka** (upar/left chhupa) | |

## Kuch important baatein
- Ye sab **read-only** hain, sirf **`scrollLeft/scrollTop` badal sakte ho** (element scroll ho jaayega).
  - `elem.scrollTop += 10` (10px neeche)
  - `scrollTop = 0` (sabse upar), `scrollTop = 1e9` (sabse neeche)
- **Hidden elements** (`display:none` ya document me nahi): saari geometry properties `0` aur `offsetParent` `null`.
```js
function isHidden(elem) { return !elem.offsetWidth && !elem.offsetHeight; }
```
- `offsetParent` `null` hota hai: hidden elements, `body/html`, aur `position:fixed` elements ke liye.
- Pura content dikhane ke liye: `element.style.height = element.scrollHeight + 'px'`

## CSS `width/height` kyun na lo (`getComputedStyle`)?
1. CSS width `box-sizing` par depend karti hai.
2. CSS width `auto` ho sakti hai (inline elements), JS ko exact `px` chahiye.
3. Scrollbar ka behavior browsers me alag (Chrome scrollbar minus karke deta hai, Firefox CSS width).
`clientWidth` in sab me consistent hai.

## Tasks ke jawab (short)
- **Neeche se kitna scroll baaki?**
  `scrollBottom = elem.scrollHeight - elem.scrollTop - elem.clientHeight`
- **Scrollbar ki width:** ek `overflow-y: scroll` wala div banao (document me daalo), `offsetWidth - clientWidth`.
- **Ball ko field ke center me:**
```js
ball.style.left = Math.round(field.clientWidth / 2 - ball.offsetWidth / 2) + 'px';
ball.style.top  = Math.round(field.clientHeight / 2 - ball.offsetHeight / 2) + 'px';
```
  Pitfall: `<img>` ki width/height nahi di to load hone tak `offsetWidth = 0` milta hai, isliye HTML/CSS me size do.
- **CSS width vs clientWidth:** `clientWidth` number hai (string nahi), `auto` nahi aata, padding include karta hai, scrollbar hamesha sahi handle karta hai.

## Yaad rakho
`offset*` = outer (border ke saath), `client*` = andar (border aur scrollbar ke bina), `scroll*` = poora content (chhupa hissa bhi).
