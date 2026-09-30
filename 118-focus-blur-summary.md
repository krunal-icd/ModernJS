# 38. Focusing: focus / blur

Element ko **focus** tab milta hai jab user click kare, `Tab` key se aaye, ya `autofocus` attribute ho.
- Focus = "yaha data lene ki taiyari" (initialize karne ka time).
- **Blur** (focus ka jaana) = "data daal diya gaya" (validate/save karne ka time).

## `focus` / `blur` events
Email validation example:
```js
input.onblur = function() {
  if (!input.value.includes('@')) {
    input.classList.add('invalid');
    error.innerHTML = 'Please enter a correct email.';
  }
};
input.onfocus = function() {
  if (this.classList.contains('invalid')) {
    this.classList.remove('invalid');
    error.innerHTML = "";
  }
};
```
(HTML attributes `required`, `pattern` se bhi validation ho sakti hai. JS tab jab zyada flexibility chahiye.)

## Methods `elem.focus()` / `elem.blur()`
Focus set/unset karte hain. Ek example me invalid value par focus wapas input par daal dete hain.

Dhyan:
- `onblur` me `preventDefault()` se focus khona **rok nahi sakte** (`onblur` focus jaane ke **baad** chalta hai).
- Practically user ko **galti dikhao, par aage badhne se mat roko** (wo pehle dusre fields bharna chah sakta hai).

**JS se focus loss ke case:**
- `alert` focus apne paas le leta hai (`blur`), band karne par wapas (`focus`).
- Element DOM se hataya to focus jaata hai, dobara insert karne par wapas nahi aata.

Isliye user-initiated focus loss track karna ho to khud focus mat hilao.

## `tabindex`: kisi bhi element ko focusable banana
Default me sirf interactive elements (`button`, `input`, `select`, `a`) focus lete hain. `div`, `span` jaise nahi. `tabindex` attribute lagao to koi bhi element focusable ban jaata hai.

- `tabindex="1"`, `"2"`,...: `Tab` ke order me pehle (chhote number se bade), phir baaki normal elements.
- `tabindex="0"`: element normal (default) order me rehta hai, par focusable ban jaata hai.
- `tabindex="-1"`: `Tab` se nahi milega, sirf `elem.focus()` se (programmatic).

Example list `1 - 2 - 0` order me chalti hai. JS me `elem.tabIndex` property bhi hai. Focus milne par `:focus` CSS se style kar sakte ho.

## Delegation: `focusin` / `focusout`
`focus` aur `blur` **bubble nahi karte**. Isliye `form.onfocus` kaam nahi karega.

2 solutions:
1. **Capturing phase**: `focus/blur` bubble nahi karte par capturing me neeche jaate hain.
```js
form.addEventListener("focus", () => form.classList.add('focused'), true);
form.addEventListener("blur", () => form.classList.remove('focused'), true);
```
2. **`focusin` / `focusout`**: bilkul `focus/blur` jaise par **bubble** karte hain. Sirf `addEventListener` se lagte hain (`on<event>` se nahi).
```js
form.addEventListener("focusin", () => form.classList.add('focused'));
form.addEventListener("focusout", () => form.classList.remove('focused'));
```

## Summary
- `focus/blur` bubble nahi karte (capturing ya `focusin/focusout` use karo).
- Zyaadatar elements default me focusable nahi, `tabindex` lagao.
- Abhi focus wala element: `document.activeElement`.

## Tasks (short)
- **Editable div:** click par `<textarea>` me badlo, `Enter` ya blur par wapas div (content HTML ban jaye).
- **TD editable:** delegation, click par cell me textarea + OK/CANCEL buttons, ek time par ek hi cell.
- **Keyboard-driven mouse:** element par `keydown` sunna ho to focusable hona chahiye, HTML change nahi kar sakte to `mouse.tabIndex = ...` JS se set karo.
