# 39. Events: change, input, cut, copy, paste

## `change` event
Jab element ne **badalna khatam** kiya.
- **Text input:** focus **khone** par chalta hai (type karte waqt nahi).
- **`select`, checkbox, radio:** selection badalte hi turant.

```html
<input type="text" onchange="alert(this.value)">
```

## `input` event
Value badalne par **har baar turant** chalta hai, chahe kaise bhi badli ho: type, mouse se paste, speech recognition.
```js
input.oninput = function() { result.innerHTML = input.value; };
```
- Input field ke **har** change ko pakadna ho to ye sabse achha.
- Keyboard events ke ulta, jo keys value nahi badalti (jaise arrow keys) unpar nahi chalta.
- **`oninput` me `preventDefault()` kaam nahi karta**, kyunki ye value badalne ke **baad** aata hai.

## `cut`, `copy`, `paste`
`ClipboardEvent` class ke events. Data `event.clipboardData` se milta hai, aur `preventDefault()` se action **roka** ja sakta hai.
```js
input.onpaste = function(event) {
  alert("paste: " + event.clipboardData.getData('text/plain'));
  event.preventDefault();
};

input.oncut = input.oncopy = function(event) {
  alert(event.type + '-' + document.getSelection());
  event.preventDefault();
};
```
- `cut/copy` handlers me `clipboardData.getData()` **khaali string** deta hai (data abhi clipboard me gaya nahi), isliye `document.getSelection()` use kiya.
- Sirf text nahi, files bhi copy/paste ho sakti hain (`clipboardData` `DataTransfer` interface hai).
- Naya async API: `navigator.clipboard` (Firefox me support nahi tha).

### Safety restrictions
Clipboard OS-level global cheez hai, isliye browsers usse **sirf user ki apni actions** (copy/paste) ke scope me access dete hain.
- `dispatchEvent` se custom clipboard events banana Firefox ke alawa sab me mana hai, aur bane bhi to clipboard access nahi milta.
- `event.clipboardData` ko save karke baad me use nahi kar sakte.
- `navigator.clipboard` kisi bhi context me use ho sakta hai, zarurat par permission maangta hai.

## Summary
| Event | Kab | Khaas baat |
|---|---|---|
| `change` | Value badli | Text input me focus khone par |
| `input` | Text input me har change | Turant, `change` ke ulta |
| `cut/copy/paste` | Cut/copy/paste | Roka ja sakta hai, `clipboardData` se data |

## Task
**Deposit calculator:** amount, percentage, years me koi bhi change turant process ho (`input` event).
```js
let result = Math.round(initial * (1 + interest) ** years);
```
