# 40. Forms: submit event aur method

`submit` event tab chalta hai jab form submit hota hai. Aam taur par bhejne se pehle **validate** karne ya JS se khud process karne ke liye use hota hai.

## Event: `submit`
Form submit hone ke 2 tarike:
1. `<input type="submit">` (ya `type="image"`) par click
2. Input field me `Enter` dabana

Dono `submit` event laate hain. Handler data check kar sakta hai, galti ho to dikhakar `event.preventDefault()` (ya `return false`) kar sakta hai, tab form server ko nahi jaata.
```html
<form onsubmit="alert('submit!'); return false">
  <input type="text" value="text">
  <input type="submit" value="Submit">
</form>
```

**`submit` aur `click` ka rishta:** input field me `Enter` se form bhejne par bhi `<input type="submit">` par ek **`click` event trigger** hota hai (jabki asal me click hua hi nahi).

## Method: `form.submit()`
JS se form bhejna:
- Isse **`submit` event generate nahi hota**. Maana jaata hai ki jo programmer ise call karta hai, usne processing pehle hi kar li.
- Khud form banake bhejne me useful:
```js
let form = document.createElement('form');
form.action = 'https://google.com/search';
form.method = 'GET';
form.innerHTML = '<input name="q" value="test">';

document.body.append(form);   // form document me hona zaruri hai
form.submit();
```

## Task: Modal form
`showPrompt(html, callback)`: ek form dikhao jisme message, input aur OK/CANCEL ho.
- `Enter` ya OK par `callback(value)`, `Esc` ya CANCEL par `callback(null)`.
- Form window ke center me, **modal** (baaki page se interaction nahi).
- Form khulte hi focus `<input>` me ho, `Tab`/`Shift+Tab` sirf form ke andar ghume.

Tarika: poore window ko dhakne wala half-transparent `<div id="cover-div">` (`position: fixed; width/height: 100%; opacity: 0.3`), ye saare clicks le leta hai. Page scroll rokne ke liye `body.style.overflowY = 'hidden'`. Form is div ke **andar nahi**, uske bagal me rakho (warna wo bhi transparent ho jayega).

## Yaad rakho
`submit` event validate/rokne ke liye, `form.submit()` JS se bhejne ke liye (event nahi aata).
