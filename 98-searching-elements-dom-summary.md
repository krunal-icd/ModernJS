# 18. Elements Dhundhna (getElement*, querySelector*)

Jab elements door door hon, to navigation ke bajay search methods use karte hain.

## `getElementById`
```js
let elem = document.getElementById('elem');
```
- Sirf `document` par chalta hai (kisi element par nahi).
- `id` **unique** honi chahiye.
- Browser `id` naam ka global variable bhi bana deta hai (jaise `elem`), par isse **avoid** karo (naming conflicts ka risk). Local JS variable ho to wo is par priority leta hai.

## `querySelectorAll(css)`
Sabse powerful. Kisi bhi **CSS selector** se **saare** matching elements deta hai.
```js
document.querySelectorAll('ul > li:last-child');
```
Pseudo-classes (`:hover`, `:active`) bhi chalte hain.

## `querySelector(css)`
Sirf **pehla** matching element (`querySelectorAll(css)[0]` jaisa, par tez).

## `elem.matches(css)`
Kuch dhundhta nahi, sirf check karta hai ki `elem` selector se match karta hai ya nahi (`true/false`). Filter karne me kaam aata hai.
```js
if (elem.matches('a[href$="zip"]')) { ... }
```

## `elem.closest(css)`
Element se **upar** ki taraf (khud element bhi) sabse paas ka **ancestor** dhundhta hai jo selector se match kare. Na mile to `null`.
```js
chapter.closest('.book');   // paas wala UL
```

## `getElementsBy*` (purane tarike)
- `getElementsByTagName(tag)` (`'*'` = sab)
- `getElementsByClassName(className)`
- `document.getElementsByName(name)`

Ab zyaadatar history, par purani scripts me milte hain.

**Galtiyan:**
- `s` mat bhulo: `getElementsByTagName` (collection), `getElementById` (ek element).
- Ye **collection** dete hain, seedha `.value = 5` nahi kar sakte. Index lagao ya loop chalao:
```js
document.getElementsByTagName('input')[0].value = 5;
```

## Live vs Static
- `getElementsBy*` = **live** collection (DOM badle to update ho jaata hai).
- `querySelectorAll` = **static** (ek fixed snapshot).

## Summary Table
| Method | Kis se dhundhta hai | Element par call? | Live? |
|---|---|---|---|
| `querySelector` | CSS selector | Haan | Nahi |
| `querySelectorAll` | CSS selector | Haan | Nahi |
| `getElementById` | id | Nahi | Nahi |
| `getElementsByName` | name | Nahi | Haan |
| `getElementsByTagName` | tag | Haan | Haan |
| `getElementsByClassName` | class | Haan | Haan |

Extra: `elemA.contains(elemB)` = `elemB`, `elemA` ke andar hai to `true` (ya dono same hon).

## Yaad rakho
Zyaadatar kaam `querySelector` aur `querySelectorAll` se ho jaata hai. `matches` filter ke liye, `closest` upar ke ancestor ke liye.
