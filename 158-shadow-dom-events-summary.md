# 78. Shadow DOM aur Events

Shadow tree ka idea component ke internal implementation details ko **encapsulate** karna hai. Maano `<user-card>` ke shadow DOM ke andar click hota hai. Par main document ki scripts ko shadow DOM internals ka koi idea nahi (khaaskar agar component kisi 3rd-party library ka ho).

Details chhupi rakhne ke liye browser event ko **retarget** karta hai.

**Jo events shadow DOM me hote hain, unka `target` component ke bahar pakadne par host element hota hai.**

```js
customElements.define('user-card', class extends HTMLElement {
  connectedCallback() {
    this.attachShadow({mode: 'open'});
    this.shadowRoot.innerHTML = `<p><button>Click me</button></p>`;
    this.shadowRoot.firstElementChild.onclick =
      e => alert("Inner target: " + e.target.tagName);
  }
});

document.onclick = e => alert("Outer target: " + e.target.tagName);
```
Button par click:
1. `Inner target: BUTTON` (andar ka handler sahi target dekhta hai)
2. `Outer target: USER-CARD` (document ka handler host ko target dekhta hai)

Retargeting achhi cheez hai kyunki bahar ke document ko component internals nahi pata hone chahiye. Uske hisaab se event `<user-card>` par hua.

**Agar event kisi slotted element par ho jo physically light DOM me rehta hai, to retargeting nahi hoti.** Jaise `<span slot="username">` par click ho to andar aur bahar dono handlers ke liye target wahi `span` hota hai (light DOM ka element, retarget nahi). Par shadow DOM ke element (jaise `<b>Name:</b>`) par click ho aur wo shadow DOM se bahar bubble kare, to uska `event.target` `<user-card>` ho jaata hai.

## Bubbling aur `event.composedPath()`
Bubbling ke liye **flattened DOM** use hota hai. Slotted element ke andar event ho to wo `<slot>` tak bubble karta hai aur phir upar.

Original target tak ka poora path (saare shadow elements ke saath) **`event.composedPath()`** deta hai (composition ke **baad** ka path).

Flattened DOM me `<span slot="username">` par click ka `composedPath()`:
`[span, slot, div, shadow-root, user-card, body, html, document, window]`

Shadow tree details sirf `{mode:'open'}` trees ke liye milte hain. `{mode: 'closed'}` ho to path host (`user-card`) se shuru hota hai (closed trees ke internals poori tarah chhupe rehte hain).

## `event.composed`
Zyaadatar events shadow DOM ki seema paar karke bubble karte hain, par kuch nahi. Ye **`composed`** property se tay hota hai. `true` ho to event boundary cross karta hai, warna sirf shadow DOM ke andar se pakda ja sakta hai.

**`composed: true`** wale (UI Events spec ke zyaadatar):
- `blur`, `focus`, `focusin`, `focusout`
- `click`, `dblclick`
- `mousedown`, `mouseup`, `mousemove`, `mouseout`, `mouseover`
- `wheel`
- `beforeinput`, `input`, `keydown`, `keyup`
- Saare touch aur pointer events

**`composed: false`** wale:
- `mouseenter`, `mouseleave` (ye bubble hi nahi karte)
- `load`, `unload`, `abort`, `error`
- `select`
- `slotchange`

Ye events sirf usi DOM ke elements par pakde ja sakte hain jahan event target hai.

## Custom events
Custom event dispatch karte waqt component ke bahar bubble karwane ke liye **`bubbles` aur `composed` dono `true`** karne padte hain:
```js
inner.dispatchEvent(new CustomEvent('test', {
  bubbles: true,
  composed: true,
  detail: "composed"
}));

inner.dispatchEvent(new CustomEvent('test', {
  bubbles: true,
  composed: false,
  detail: "not composed"
}));
```
Sirf `composed: true` wala document tak pahunchta hai.

## Summary
Events shadow DOM ki seema tabhi paar karte hain jab unka `composed` flag `true` ho.

Built-in events me zyaadatar `composed: true` hota hai (UI Events, Touch Events, Pointer Events specs). `composed: false` wale: `mouseenter`, `mouseleave`, `load`, `unload`, `abort`, `error`, `select`, `slotchange`.

`CustomEvent` dispatch karo to `composed: true` **khud set** karo.

Nested components me ek shadow DOM dusre ke andar ho sakta hai. Tab composed events saari shadow DOM seemaon se bubble hote hain. Agar event sirf sabse paas ke enclosing component ke liye chahiye, to use **shadow host par dispatch** karo aur `composed: false` rakho. Tab wo component ke shadow DOM se bahar niklega par upar ke DOM tak bubble nahi karega.
