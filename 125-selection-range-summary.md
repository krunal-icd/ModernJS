# 45. Selection aur Range

Document me selection (aur `<input>` jaise form fields me selection) ko JS se padh sakte hain, set kar sakte hain, hata sakte hain, tag me wrap kar sakte hain, etc.

## `Range`
Selection ka basic concept: do **boundary points** (start aur end).
```js
let range = new Range();
range.setStart(node, offset);
range.setEnd(node, offset);
```
`node` text node ya element node ho sakta hai, aur `offset` ka matlab uske hisaab se badalta hai:

- **Text node:** `offset` = text me **position** (characters).
```js
// <p id="p">Hello</p>
range.setStart(p.firstChild, 2);
range.setEnd(p.firstChild, 4);
console.log(range);   // "ll"
```
- **Element node:** `offset` = **child ka number** (poore nodes select karne ke liye).
```js
// <p id="p">Example: <i>italic</i> and <b>bold</b></p>
range.setStart(p, 0);
range.setEnd(p, 2);   // 2 tak, 2 include nahi
// range = "Example: italic"
```
Start aur end alag alag nodes me bhi ho sakte hain (bas end, start ke baad ho).

### Range properties
- `startContainer`, `startOffset`
- `endContainer`, `endOffset`
- `collapsed`: start = end (koi content nahi)
- `commonAncestorContainer`: sab nodes ka sabse paas ka common ancestor

### Range set karne ke methods
- `setStart(node, offset)`, `setStartBefore(node)`, `setStartAfter(node)`
- `setEnd(node, offset)`, `setEndBefore(node)`, `setEndAfter(node)`
- `selectNode(node)`: poora node select
- `selectNodeContents(node)`: node ke andar ka poora content
- `collapse(toStart)`: range ko collapse karo
- `cloneRange()`: same start/end ka naya range

### Range editing methods
- `deleteContents()`: content document se hatao
- `extractContents()`: hatao aur `DocumentFragment` me lo
- `cloneContents()`: clone karke `DocumentFragment` me lo
- `insertNode(node)`: range ki shuruaat me node daalo
- `surroundContents(node)`: node me content ko wrap karo (range me dono opening aur closing tags hone chahiye, `<i>abc` jaise partial nahi)

## `Selection`
`Range` sirf ek object hai, isse screen par kuch select nahi hota. Document ka selection **`Selection`** object hai: `window.getSelection()` ya `document.getSelection()`.
- Theory me ek se zyada ranges ho sakti hain (sirf **Firefox** me `Ctrl+click` se), baaki browsers me max **1**.

### Selection properties
- `anchorNode`, `anchorOffset`: selection **kahan se shuru**
- `focusNode`, `focusOffset`: kahan **khatam**
- `isCollapsed`: kuch select nahi
- `rangeCount`: ranges ki ginti (Firefox ke alawa max 1)
- `getRangeAt(i)`: i-th range

**Farak:** `Range` ka start hamesha end se pehle hota hai, par selection me nahi. Mouse se left-to-right ya **right-to-left** dono tarah select ho sakta hai, isliye `focus`, `anchor` se pehle bhi ho sakta hai.

### Selection events
- `elem.onselectstart`: selection is element par **shuru** ho. Isme default action roko to yaha se selection shuru nahi hoga.
- `document.onselectionchange`: selection badle ya shuru ho (sirf `document` par).

### Copy karna
1. Text: `document.getSelection().toString()`
2. DOM (formatting ke saath): `selection.getRangeAt(i).cloneContents()` (DocumentFragment)

### Selection methods
- `addRange(range)`, `removeRange(range)`, `removeAllRanges()`, `empty()`
- `collapse(node, offset)` / `setPosition`, `collapseToStart()`, `collapseToEnd()`
- `extend(node, offset)`: focus ko hilao
- `setBaseAndExtent(anchorNode, anchorOffset, focusNode, focusOffset)`: selection set karo
- `selectAllChildren(node)`
- `deleteFromDocument()`: selected content hatao
- `containsNode(node, allowPartialContainment)`

Poora `<p>` ka content select:
```js
document.getSelection().setBaseAndExtent(p, 0, p, p.childNodes.length);
// ya
let range = new Range();
range.selectNodeContents(p);
document.getSelection().removeAllRanges();   // pehle purana hatao
document.getSelection().addRange(range);
```
**Naya select karne se pehle `removeAllRanges()`** karo, warna Firefox ke alawa browsers naya range ignore kar dete hain. (`setBaseAndExtent` jaise methods jo khud replace karte hain, unme zarurat nahi.)

## Form controls me selection (`input`, `textarea`)
Alag simple API (kyunki value plain text hai):
- `input.selectionStart`, `selectionEnd` (writable), `selectionDirection`
- Event: `input.onselect`
- Methods:
  - `input.select()`: sab select
  - `setSelectionRange(start, end, [direction])`
  - `setRangeText(replacement, [start], [end], [selectionMode])`: text replace. `selectionMode`: `"select"`, `"start"`, `"end"`, `"preserve"` (default).

**Cursor move karna:** `selectionStart = selectionEnd = 10` (dono barabar = cursor us position par).
```js
area.onfocus = () => {
  setTimeout(() => { area.selectionStart = area.selectionEnd = 10; });  // zero-delay setTimeout
};
```

**Selection ko `*...*` me wrap:**
```js
let selected = input.value.slice(input.selectionStart, input.selectionEnd);
input.setRangeText(`*${selected}*`);
```

**Cursor par text daalna:**
```js
input.setRangeText("HELLO", input.selectionStart, input.selectionEnd, "end");
input.focus();
```

## Unselectable banana (3 tarike)
1. CSS: `user-select: none` (selection yaha se shuru nahi hoga, par dusri jagah se extend karke shamil ho sakta hai, copy me aam taur par ignore hota hai)
2. `onselectstart` ya `mousedown` me default action roko (`elem.onselectstart = () => false`)
3. Baad me `document.getSelection().empty()` (kam use, blink karta hai)

## Summary
- Document: `Selection` + `Range` objects. Form fields: `selectionStart/End`, `setRangeText` etc.
- Selection lena: `getSelection()` aur `getRangeAt(i).cloneContents()`.
- Selection set karna: `setBaseAndExtent(...)` ya `removeAllRanges()` + `addRange(range)`.
- Cursor position = selection ka start/end.
