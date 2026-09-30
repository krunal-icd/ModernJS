# 75. Template Element

Built-in **`<template>`** element HTML markup templates ka **storage** hai. Browser iske content ko ignore karta hai (sirf syntax valid hai ya nahi check karta hai), par hum JS me use access karke dusre elements bana sakte hain.

Theory me koi bhi invisible element bana ke markup rakh sakte hain. `<template>` me khaas kya hai?

**1. Content koi bhi valid HTML ho sakta hai**, chahe wo normally sahi wrapper tag maangta ho. Jaise table row `<tr>`:
```html
<template>
  <tr>
    <td>Contents</td>
  </tr>
</template>
```
Normally `<tr>` ko `<div>` me rakho to browser use "fix" karke `<table>` laga deta hai. `<template>` jaisa daalo waisa hi rakhta hai.

**2. Styles aur scripts bhi** rakh sakte hain:
```html
<template>
  <style> p { font-weight: bold; } </style>
  <script> alert("Hello"); </script>
</template>
```

**3.** Browser `<template>` ka content **"document ke bahar"** maanta hai: styles nahi lagte, scripts nahi chalte, `<video autoplay>` nahi chalta. Content tab **"live"** hota hai (styles lagte, scripts chalte) jab use document me daalo.

## Template insert karna
Template ka content `content` property me hai, ek **`DocumentFragment`** (special DOM node). Isse normal DOM node ki tarah treat kar sakte ho, bas ek khaas baat: jahan insert karo wahan **uske children** insert hote hain.
```html
<template id="tmpl">
  <script> alert("Hello"); </script>
  <div class="message">Hello, world!</div>
</template>

<script>
  let elem = document.createElement('div');

  elem.append(tmpl.content.cloneNode(true));   // clone karo, taaki kai baar reuse ho sake

  document.body.append(elem);
  // ab template ki script chalti hai
</script>
```

## Shadow DOM ke saath
Pichhle chapter ka example `<template>` se:
```html
<template id="tmpl">
  <style> p { font-weight: bold; } </style>
  <p id="message"></p>
</template>

<div id="elem">Click me</div>

<script>
  elem.onclick = function() {
    elem.attachShadow({mode: 'open'});
    elem.shadowRoot.append(tmpl.content.cloneNode(true));   // (*)
    elem.shadowRoot.getElementById('message').innerHTML = "Hello from the shadows!";
  };
</script>
```
`(*)` par `tmpl.content` (DocumentFragment) clone karke insert karne par uske children (`<style>`, `<p>`) insert hote hain, aur wo shadow DOM banate hain.

## Summary
- `<template>` ka content koi bhi syntactically sahi HTML ho sakta hai.
- Ye "document se bahar" maana jaata hai, isliye kisi par asar nahi karta.
- JS me `template.content` se access karke clone karke naye component me reuse kar sakte hain.

`<template>` alag isliye hai ki:
- Browser andar ke HTML ka **syntax check** karta hai (script ke andar template string me nahi hota).
- Phir bhi kisi bhi top-level HTML tag ki ijazat (jaise `<tr>` jo bina wrapper ke bekaar hai).
- Document me insert hone par content **interactive** hota hai (scripts chalti hain, `<video autoplay>` chalta hai).

`<template>` me iteration, data binding ya variable substitution jaise mechanisms **nahi** hain, par unhe uske upar khud bana sakte hain.
