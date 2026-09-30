# 67. localStorage aur sessionStorage

Web storage objects browser me **key/value** pairs save karte hain. Data page refresh (`sessionStorage`) aur browser restart (`localStorage`) ke baad bhi bacha rehta hai.

## Cookies ke hote hue ye kyun?
- Cookies ke ulta ye **har request ke saath server ko nahi bheje** jaate, isliye **bahut zyada** data (aam taur par **5MB+**) rakh sakte hain.
- Server HTTP headers se inhe **badal nahi** sakta. Sab JS me hota hai.
- **Origin** (domain/protocol/port) se bandhe hote hain. Alag protocol ya subdomain ka alag storage.

## Dono ke same methods
- `setItem(key, value)`
- `getItem(key)`
- `removeItem(key)`
- `clear()`
- `key(index)`: given position ki key
- `length`: kitne items

`Map` jaisa hai, par `key(index)` se index se bhi milta hai.

## `localStorage`
- Same origin ke **saare tabs aur windows me shared**.
- Data **expire nahi hota**. Browser restart, OS reboot ke baad bhi.
```js
localStorage.setItem('test', 1);
alert(localStorage.getItem('test'));   // 1 (browser band karke phir bhi)
```
Origin same hona chahiye, URL path alag ho sakta hai. Ek window me set karo to doosri me dikhta hai.

## Object jaisa access
```js
localStorage.test = 2;
alert(localStorage.test);
delete localStorage.test;
```
Historical wajah se chalta hai, par **recommend nahi**:
1. Key user-generated ho (jaise `length`, `toString`) to fail: `localStorage['length'] = 5` error deta hai. `getItem/setItem` theek rehte hain.
2. `storage` event object-like access par **trigger nahi** hota.

## Keys par loop
Storage objects **iterable nahi** hain.
```js
for (let i = 0; i < localStorage.length; i++) {
  let key = localStorage.key(i);
  alert(`${key}: ${localStorage.getItem(key)}`);
}

// ya
let keys = Object.keys(localStorage);
for (let key of keys) { ... }
```
`for key in localStorage` built-in fields (`getItem`, `setItem`...) bhi deta hai, to `hasOwnProperty` se filter karna padta hai. `Object.keys` seedha sirf apni keys deta hai.

## Sirf strings
Key aur value **string** honi chahiye. Number/object ho to string me badal jaata hai:
```js
localStorage.user = {name: "John"};
alert(localStorage.user);   // [object Object]
```
Objects ke liye `JSON`:
```js
localStorage.user = JSON.stringify({name: "John"});
let user = JSON.parse(localStorage.user);
```
Debug ke liye poora storage: `JSON.stringify(localStorage, null, 2)`.

## `sessionStorage`
`localStorage` se kam use hota hai. Methods same, par zyada limited:
- Sirf **current browser tab** me hota hai.
  - Same page ke dusre tab ka alag storage.
  - Par same tab ke same-origin **iframes ke beech shared**.
- Data **page refresh** se bachta hai, par **tab band/khole** jaane par nahi.

```js
sessionStorage.setItem('test', 1);
// refresh ke baad: sessionStorage.getItem('test') -> 1
// dusre tab me: null
```

## `storage` event
`localStorage`/`sessionStorage` badalne par chalta hai. Properties:
- `key`: kaunsi key badli (`clear()` par `null`)
- `oldValue`: purani value (nayi key par `null`)
- `newValue`: nayi value (hatane par `null`)
- `url`: jis document me update hua uska URL
- `storageArea`: `localStorage` ya `sessionStorage`

**Important:** event storage access karne wali **saari window objects par chalta hai, siwaye us ke jisne badlaav kiya.**
```js
window.onstorage = event => {
  if (event.key != 'now') return;
  alert(event.key + ':' + event.newValue + " at " + event.url);
};
localStorage.setItem('now', Date.now());
```
Isse same origin ki alag windows aapas me **messages exchange** kar sakti hain. (Modern browsers me **Broadcast Channel API** bhi hai, jyada features par kam supported.)

## Summary
- Key aur value strings, limit 5MB+, expire nahi hote, origin se bandhe.

| `localStorage` | `sessionStorage` |
|---|---|
| Same origin ke sab tabs/windows me shared | Ek tab (aur uske same-origin iframes) me |
| Browser restart ke baad bhi | Page refresh ke baad bhi (tab band hone ke baad nahi) |

- Sabhi keys ke liye `Object.keys`.
- Object-style access par `storage` event nahi aata.
- `storage` event `setItem`, `removeItem`, `clear` par, aur jisne badla us window ko chhodkar baaki par.

## Task: Form autosave
Ek `textarea` jo har change par apni value `localStorage` me save kare (`input` event), taaki page band hokar dobara khule to adhoora likha wapas mile.
