# 17. Walking the DOM (DOM me ghumna)

Saare operations `document` se shuru hote hain. Isse kisi bhi node tak pahunch sakte ho.

## Sabse upar
| Element | Kaise milega |
|---|---|
| `<html>` | `document.documentElement` |
| `<body>` | `document.body` |
| `<head>` | `document.head` |

**Dhyan:** agar script `<head>` me hai, to `document.body` `null` hoga (abhi body parse nahi hui). DOM me `null` = "exist nahi karta".

## Terms
- **Children**: seedhe andar wale nodes.
- **Descendants**: saare nested nodes (children ke children bhi).

## Sab nodes ke liye (text/comment bhi included)
| Property | Kaam |
|---|---|
| `childNodes` | saare child nodes |
| `firstChild`, `lastChild` | pehla aur aakhri child |
| `parentNode` | parent |
| `previousSibling`, `nextSibling` | bagal wale nodes |
| `hasChildNodes()` | child hai ya nahi |

## Sirf Elements ke liye (text/comment skip)
| Property | Kaam |
|---|---|
| `children` | sirf element children |
| `firstElementChild`, `lastElementChild` | |
| `previousElementSibling`, `nextElementSibling` | |
| `parentElement` | parent element |

`parentNode` vs `parentElement`: sirf `document.documentElement` (`<html>`) me fark hai. Uska `parentNode` = `document`, par `parentElement` = `null`.
```js
while (elem = elem.parentElement) { /* <html> tak upar jao */ }
```

## DOM Collections
`childNodes` **array nahi**, ek collection hai (array-like, iterable).
- `for..of` chalta hai. `for..in` **mat** use karo (extra properties aati hain).
- Array methods (`filter` etc.) nahi chalte. `Array.from(collection)` se array bana lo.
- **Read-only** hain (`childNodes[i] = ...` nahi kar sakte).
- Zyaadatar **live** hote hain (DOM badle to apne aap update).

## Tables ke special properties
- `table.rows`, `table.tBodies`, `table.caption`, `table.tHead`, `table.tFoot`
- `tbody.rows`
- `tr.cells`, `tr.rowIndex`, `tr.sectionRowIndex`
- `td.cellIndex`
```js
let td = table.rows[0].cells[1]; // pehli row, doosra column
td.style.backgroundColor = "red";
```

## Tasks se seekh
- `elem.lastChild.nextSibling` hamesha `null` hota hai (sahi).
- `elem.children[0].previousSibling` hamesha `null` **nahi** hota, kyunki pehle text node ho sakta hai.

## Yaad rakho
Sab nodes wale set: `parentNode, childNodes, firstChild, lastChild, previousSibling, nextSibling`.
Sirf elements wale set: `parentElement, children, firstElementChild, lastElementChild, previousElementSibling, nextElementSibling`.
