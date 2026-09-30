# 15. Browser Environment aur Specs

## Host Environment
JS alag alag jagah chalti hai (browser, Node.js server, etc.). Har jagah ko **host environment** kehte hain, jo language ke core ke alawa apne extra objects/functions deta hai. Browser page control karne ke tools deta hai, Node.js server-side features.

## `window` (root object)
Browser me iske do kaam:
1. JS ka **global object** (global functions = `window` ke methods).
2. **Browser window** ka representation.
```js
function sayHi() { alert("Hello"); }
window.sayHi();

alert(window.innerHeight);  // window ki height
```

## DOM (Document Object Model)
- Page ka poora content **objects** ke roop me. Inhe modify kar sakte ho.
- `document` object entry point hai.
```js
document.body.style.background = "red";
```
- DOM sirf browser ke liye nahi, server-side tools bhi use karte hain.

## CSSOM
CSS rules/stylesheets ko objects ke roop me dikhata hai. Zyaadatar hum sirf **CSS classes add/remove** karte hain, isliye CSSOM kam kaam aata hai.

## BOM (Browser Object Model)
Document ke alawa browser ke baaki objects:
- `navigator`: browser aur OS ki info (`navigator.userAgent`, `navigator.platform`)
- `location`: current URL padhna aur redirect karna
```js
alert(location.href);
location.href = "https://wikipedia.org";
```
- `alert / confirm / prompt` bhi BOM ka hissa hain.

## Specifications
| Spec | Kya describe karta hai |
|---|---|
| DOM spec (dom.spec.whatwg.org) | Document structure, manipulation, events |
| CSSOM spec | Stylesheets aur style rules |
| HTML spec (html.spec.whatwg.org) | HTML language + BOM (`setTimeout`, `alert`, `location` etc.) |

Kuch chahiye to search karo: **"WHATWG [term]"** ya **"MDN [term]"**.

## Yaad rakho
Browser me JS = **window** (root) -> **DOM** (`document`) + **BOM** (`navigator`, `location`, ...) + **CSSOM**. Ab hum DOM seekhenge kyunki UI me document sabse important hai.
