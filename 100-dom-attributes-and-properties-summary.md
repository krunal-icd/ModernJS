# 20. Attributes aur Properties

## Do alag cheezein
- **Attribute** = jo **HTML me likha** hai.
- **Property** = jo **DOM object me** hai.

Browser HTML parse karke DOM banata hai, aur **standard** attributes ke liye automatically properties bana deta hai (`<body id="page">` -> `body.id === "page"`). Par ye mapping hamesha ek jaisi nahi hoti.

## DOM Properties
DOM nodes normal JS objects hain, apni property/method jod sakte ho:
```js
document.body.myData = { name: 'Caesar' };
Element.prototype.sayHi = function() { alert(this.tagName); };
```
Ye kuch bhi value ho sakti hain aur **case-sensitive** hain.

## HTML Attributes
- **Non-standard** attribute ke liye property nahi banti:
```html
<body id="test" something="non-standard">
```
`document.body.something` -> `undefined`.

Attributes ke saath kaam karne ke methods:
- `elem.hasAttribute(name)`
- `elem.getAttribute(name)`
- `elem.setAttribute(name, value)`
- `elem.removeAttribute(name)`
- `elem.attributes`: sab attributes ka iterable collection

Attributes ki khasiyat:
- Naam **case-insensitive** (`id` = `ID`)
- Value hamesha **string** (`setAttribute('Test', 123)` -> `"123"`)
- Sab `outerHTML` me dikhte hain

## Property aur Attribute ka sync
Zyaadatar dono taraf sync hota hai:
```js
input.setAttribute('id', 'id');   // input.id = 'id'
input.id = 'newId';               // getAttribute('id') = 'newId'
```
**Exception:** `input.value` sirf attribute -> property sync hota hai, ulta nahi:
```js
input.setAttribute('value', 'text');  // input.value = 'text'
input.value = 'newValue';             // attribute 'text' hi rehta hai
```
Ye useful hai: user badal de to bhi HTML ki original value attribute me bachi rehti hai.

## Properties typed hoti hain
Zyaadatar string, par kabhi kabhi nahi:
- `checkbox.checked`: **boolean** (attribute wahi empty string)
- `style` attribute = string, `style` property = **object** (`div.style.color`)
- `href` property hamesha **poora URL** deti hai, attribute wahi jo HTML me likha (`#hello`). "Jaisa likha waisa" chahiye to `getAttribute('href')`.

## Custom data: `data-*` aur `dataset`
Non-standard attribute future me standard ban sakta hai (conflict ka risk). Isliye **`data-`** se shuru hone wale attributes programmers ke liye reserved hain:
```html
<div id="order" data-order-state="new"></div>
```
```js
order.dataset.orderState;             // "new" (camelCase)
order.dataset.orderState = "pending"; // badalna bhi allowed
```
CSS me bhi use kar sakte ho: `.order[data-order-state="new"] { color: green; }`

## Summary
| | Properties | Attributes |
|---|---|---|
| Type | Kuch bhi (standard ke typed) | Sirf string |
| Naam | Case-sensitive | Case-insensitive |

- Zyaadatar cases me **properties** use karo.
- **Attributes** tab use karo jab: non-standard attribute chahiye (`data-*` ho to `dataset`), ya HTML me likhi original value chahiye.

## Tasks se
- `data-widget-name` padhna: `elem.dataset.widgetName`
- External links dhundhna: `a[href*="://"]:not([href^="http://internal.com"])`. Yaha `getAttribute('href')` use hota hai (property nahi) kyunki raw value chahiye.
