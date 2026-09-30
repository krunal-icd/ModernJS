# 37. Form Properties aur Methods

## Forms ke elements tak pahunchna
- `document.forms` ek **named collection** hai (naam ya number dono se):
```js
document.forms.my;    // <form name="my">
document.forms[0];    // pehla form
```
- Form ke andar ke elements: `form.elements` (naam ya index se):
```js
let form = document.forms.my;
let elem = form.elements.one;
elem.value;
```
- Same naam ke kai elements (radio/checkbox) ho to `form.elements[name]` ek **collection** deta hai.
- `form.elements` tag ke nesting par depend nahi karta, kitne bhi andar ho, mil jaate hain.
- `<fieldset>` ka bhi `elements` property hota hai (ek tarah ka "sub-form").
- **Chhota tarika:** `form.login` = `form.elements.login`. (Naam badalne par purana aur naya dono naam kaam karte hain.)

## Back reference: `element.form`
Kisi bhi element se uska form: `element.form`.

## Form elements

**`input` aur `textarea`**
```js
input.value = "New value";
textarea.value = "New text";
input.checked = true;    // checkbox/radio ke liye (boolean)
```
`textarea.innerHTML` mat use karo (wo sirf shuruaati HTML dikhata hai, current value nahi).

**`select` aur `option`**
- `select.options`: `<option>` ka collection
- `select.value`: selected option ki value
- `select.selectedIndex`: selected option ka number (0 se shuru)

Value set karne ke 3 tarike (teeno same kaam karte hain):
```js
select.options[2].selected = true;
select.selectedIndex = 2;
select.value = 'banana';
```
`multiple` attribute ho to pehla tarika (har option ka `selected`) use karo:
```js
let selected = Array.from(select.options)
  .filter(option => option.selected)
  .map(option => option.value);
```

**`new Option`**
```js
let option = new Option(text, value, defaultSelected, selected);
```
- `defaultSelected` = `selected` HTML attribute banata hai
- `selected` = option abhi selected hai ya nahi
- Aam taur par dono ek jaise rakho (ya chhod do, default `false`).

Option properties: `option.selected`, `option.index`, `option.text`.

## Summary
- `document.forms[name/index]`
- `form.elements[name/index]` (ya `form[name]`), `fieldset` par bhi chalta hai
- `element.form`
- Value: `input.value`, `textarea.value`, `select.value`; checkbox/radio ke liye `checked`; select ke liye `selectedIndex`, `options`.

## Task
`<select>` me nayi option jodna aur select karna:
```js
let selectedOption = genres.options[genres.selectedIndex];  // abhi selected
let newOption = new Option("Classic", "classic");
genres.append(newOption);
newOption.selected = true;
```
