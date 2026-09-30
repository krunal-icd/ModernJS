# 22. Styles aur Classes

## Ek important rule
Element style karne ke 2 tarike: **CSS class** ya seedha **`style`**.
**Hamesha CSS classes ko prefer karo.** `style` tab use karo jab classes se na ho sake (jaise runtime me calculate hue coordinates).
```js
elem.style.left = left;   // e.g. '123px', runtime pe calculate hui
elem.style.top = top;
```

## `className` aur `classList`
- `elem.className`: poori `class` string (assign karne par **saari classes replace** ho jaati hain).
- `elem.classList`: ek object jo individual classes ke liye methods deta hai:
  - `add("x")`, `remove("x")`
  - `toggle("x")`: hai to hata do, nahi hai to lagao
  - `contains("x")`: `true/false`
  - Ye iterable bhi hai (`for..of`).
```js
document.body.classList.add('article');
```

## `elem.style`
- `style` attribute jaisa object. Multi-word properties **camelCase** me:
  - `background-color` -> `style.backgroundColor`
  - `z-index` -> `style.zIndex`
  - `border-left-width` -> `style.borderLeftWidth`
  - Prefixed: `-webkit-border-radius` -> `style.WebkitBorderRadius`

## Style reset karna
`delete` nahi, **empty string** assign karo:
```js
elem.style.display = "none";   // hide
elem.style.display = "";       // wapas normal (CSS classes lag jaati hain)
// ya
elem.style.removeProperty('background');
```

## `style.cssText`
Poora style ek string me set karna (`!important` bhi de sakte ho). Par ye **purane sab inline styles ko replace** kar deta hai, isliye kam use hota hai.
```js
div.style.cssText = `color: red !important; width: 100px;`;
```
(`div.style = "..."` **nahi** chalta, `style` read-only object hai.)

## Units mat bhulo!
```js
document.body.style.margin = 20;     // kaam nahi karta (ignore)
document.body.style.margin = '20px'; // sahi
```
Browser `margin` ko todkar `marginTop`, `marginLeft` etc. bhi nikal deta hai.

## `getComputedStyle` (style **padhne** ke liye)
`elem.style` sirf `style` attribute ko dekhta hai, **CSS classes/cascade nahi**. Isliye:
```js
let cs = getComputedStyle(document.body);
cs.marginTop;   // "5px"
cs.color;       // "rgb(255, 0, 0)"
```
Syntax: `getComputedStyle(element, [pseudo])` (jaise `'::before'`).

Kuch notes:
- Aajkal ye **resolved value** deta hai (relative units jaise `1em` ka fixed `px` version).
- **Poora property naam** do (`paddingLeft`), shorthand (`padding`) ka result guaranteed nahi.
- `:visited` link ke styles **chhupe** rahte hain (privacy ke liye).

## Summary
- Classes: `className` (poora set), `classList` (individual).
- Styles likhna: `style.property`, `style.cssText`.
- Styles padhna (final): `getComputedStyle(elem)`, read-only.
