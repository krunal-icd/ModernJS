# 19. Node Properties: type, tag aur contents

## DOM Node Classes (hierarchy)
Har DOM node kisi built-in class ka hota hai, aur classes ek chain banati hain:

`EventTarget` -> `Node` -> `Element` -> `HTMLElement` -> (`HTMLInputElement`, `HTMLBodyElement`, `HTMLAnchorElement`, ...)

Aur `Node` -> `Document`, `Node` -> `CharacterData` -> (`Text`, `Comment`).

- `EventTarget`: events support ka base.
- `Node`: tree functionality (`parentNode`, `childNodes`, ...).
- `Element`: element navigation aur search (`children`, `querySelector`).
- `HTMLElement`: sab HTML elements ki base.

Kisi node ke properties = poori inheritance chain ka jod. Check karne ke tarike:
```js
document.body.constructor.name;          // HTMLBodyElement
document.body instanceof HTMLElement;    // true
console.dir(elem);   // properties dekhne ke liye (console.log tree dikhata hai)
```

## `nodeType` (purana tarika, number deta hai)
- `1` = element, `3` = text, `9` = document.
- Read-only.

## `nodeName` vs `tagName`
- `tagName`: sirf **elements** ke liye.
- `nodeName`: **kisi bhi node** ke liye (text ke liye `#text`, comment ke liye `#comment`, document ke liye `#document`).
- HTML me tag name hamesha **UPPERCASE** (`BODY`).

## `innerHTML`
Element ke andar ka HTML string. Padh bhi sakte ho, badal bhi.
```js
document.body.innerHTML = 'The new BODY!';
```
- Galat HTML browser theek kar deta hai.
- `<script>` insert karoge to **execute nahi hota**.
- **`innerHTML += "..."` append nahi, poora overwrite hai**: purana content hata kar naya likhta hai. Images reload hoti hain, input me likha text ud jaata hai, selection hat jaati hai.

## `outerHTML`
Element ka poora HTML (khud element bhi). Dhyan: **write karne par element badalta nahi, DOM se hat kar naye HTML se replace ho jaata hai.** Purana variable purani value hi rakhta hai.

## `nodeValue` / `data`
Text aur comment nodes ka content (`innerHTML` sirf elements ke liye hai). Aam taur par `data` use karte hain.
```js
let text = document.body.firstChild;
text.data;
```

## `textContent`
Element ka sirf **text**, saare tags hata ke.
- Padhna kam kaam aata hai.
- **Likhna bahut useful hai**: text ko safely daalta hai, tags ko literal text samajhta hai. User ke diye input ke liye ye safe hai (XSS se bachata hai).
```js
elem2.textContent = "<b>hi</b>";   // literally "<b>hi</b>" dikhega
```

## `hidden`
`elem.hidden = true` = `display:none` jaisa.

## Aur properties (class ke hisaab se)
`value` (input/select/textarea), `href` (a), `id` (sab), `type` etc.

## Tasks ke jawab (short)
- `document.body.lastChild.nodeType` script ke andar chalane par `1` (script khud aakhri node hai, baaki page abhi parse nahi hua).
- `body.innerHTML = "<!--" + body.tagName + "-->"` ke baad `body.firstChild.data` = `"BODY"`.
- `document` ki class `HTMLDocument` hai (chain: `HTMLDocument` -> `Document` -> `Node`).

## Yaad rakho
`innerHTML` (HTML string, overwrite dhyan se), `outerHTML` (replace hota hai), `textContent` (safe text), `data` (text/comment), `nodeType`, `nodeName/tagName`, `hidden`.
