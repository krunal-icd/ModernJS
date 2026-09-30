# 32. Mouse Moving: mouseover/out, mouseenter/leave

## `mouseover` / `mouseout` aur `relatedTarget`
Ye events me ek extra property hoti hai: **`relatedTarget`** (`target` ka jodidar).

| Event | `target` | `relatedTarget` |
|---|---|---|
| `mouseover` | jis element par aaye | jis element se aaye |
| `mouseout` | jis element ko chhoda | jis element par gaye |

`relatedTarget` `null` bhi ho sakta hai (mouse window ke bahar se aaya ya bahar gaya). Isliye `event.relatedTarget.tagName` seedha mat likho, error aa sakta hai.

## Elements skip ho sakte hain
Browser mouse ki position **kabhi kabhi** check karta hai (har pixel par nahi). Tez move karne par beech ke elements **skip** ho sakte hain. Par ek baat pakki hai: agar `mouseover` aaya hai to element chhodne par `mouseout` **zaroor** aayega.

## Child par jaane par bhi `mouseout`!
Browser ke hisaab se pointer ek time par sirf **ek** element (sabse nested aur upar wala) par hota hai. To agar `#parent` se `#child` (uska descendant) me jao:
1. `#parent` par `mouseout` (target = parent)
2. `#parent` par `mouseover` (child se **bubble** hokar, target = child)

Aisa lagta hai ki pointer parent se nikal ke wapas aaya, par asal me wo abhi bhi parent ke andar hi hai. Agar leave par animation chalti hai, to ye unwanted ho sakta hai. Fix: `relatedTarget` check karo, ya `mouseenter/leave` use karo.

## `mouseenter` / `mouseleave`
`mouseover/out` jaise hi, par 2 farak:
1. **Descendants ke andar ki movement count nahi hoti** (sirf poore element me aana/jaana).
2. **Bubble nahi karte.**

Bahut simple hain, par bubble na karne ki wajah se **delegation nahi** kar sakte.

## Delegation ke liye: `mouseover/out` + filtering
Table ke saare `<td>` ke liye ek handler `<table>` par lagana ho to `mouseover/out` use karo aur kaam ke events chhaanke:
```js
let currentElem = null;

table.onmouseover = function(event) {
  if (currentElem) return;                     // abhi td ke andar hi hain
  let target = event.target.closest('td');
  if (!target) return;                         // td me nahi gaye
  if (!table.contains(target)) return;         // nested table ka td
  currentElem = target;
  onEnter(currentElem);
};

table.onmouseout = function(event) {
  if (!currentElem) return;
  let relatedTarget = event.relatedTarget;
  while (relatedTarget) {
    if (relatedTarget == currentElem) return;  // td ke andar hi (descendant par) ja rahe hain
    relatedTarget = relatedTarget.parentNode;
  }
  onLeave(currentElem);                        // sach me td chhod diya
  currentElem = null;
};
```
Isse sirf `<td>` me aana/jaana pakda jaata hai, andar ke tags ki movement ignore hoti hai.

## Summary
- Tez move me beech ke elements skip ho sakte hain.
- `mouseover/out` parent se child jaane par bhi chalte hain, `mouseenter/leave` nahi.
- `mouseenter/leave` bubble nahi karte.

## Tasks (short)
- **Nested tooltip:** `data-tooltip` wale nested elements me sabse andar wala dikhao (`mouseover` par `closest('[data-tooltip]')`).
- **Smart tooltip (HoverIntent):** tooltip tab dikhao jab mouse element **par ruk** jaye ya dheere chale, tez guzar jaye to nahi. Mouse ki current position seedha nahi milti, isliye `mousemove` se coordinates yaad rakho aur har ~100ms par doori compare karo.
