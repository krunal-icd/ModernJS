# 59. Fetch API (saare options)

> Ye zyaadatar options kam use hote hain. Skip karke bhi `fetch` achhe se use kar sakte ho. Par pata hona achha hai.

Saare options unki default values ke saath:
```js
fetch(url, {
  method: "GET",                       // POST, PUT, DELETE, etc.
  headers: { "Content-Type": "text/plain;charset=UTF-8" },
  body: undefined,                     // string, FormData, Blob, BufferSource, URLSearchParams
  referrer: "about:client",            // "" ya same-origin URL
  referrerPolicy: "strict-origin-when-cross-origin",
  mode: "cors",                        // same-origin, no-cors
  credentials: "same-origin",          // omit, include
  cache: "default",                    // no-store, reload, no-cache, force-cache, only-if-cached
  redirect: "follow",                  // manual, error
  integrity: "",                       // jaise "sha256-abcdef1234567890"
  keepalive: false,                    // true
  signal: undefined,                   // AbortController
  window: window                       // null
});
```
`method`, `headers`, `body` (fetch chapter) aur `signal` (abort chapter) pehle cover ho chuke.

## `referrer` aur `referrerPolicy`
`Referer` HTTP header kaise set ho.
- **`referrer`**: exact `Referer` set karo (sirf current origin ke andar) ya hata do:
```js
fetch('/page', { referrer: "" });   // koi Referer header nahi
```
- **`referrerPolicy`**: general rules. 3 tarah ki requests: same origin, dusra origin, HTTPS -> HTTP.

| Value | Same origin | Another origin | HTTPS->HTTP |
|---|---|---|---|
| `no-referrer` | - | - | - |
| `no-referrer-when-downgrade` | full | full | - |
| `origin` | origin | origin | origin |
| `origin-when-cross-origin` | full | origin | origin |
| `same-origin` | full | - | - |
| `strict-origin` | origin | origin | - |
| `strict-origin-when-cross-origin` (default) | full | origin | - |
| `unsafe-url` | full | full | full |

(full = poora URL, origin = sirf `https://site.com`, `-` = kuch nahi)

Example: admin area ka path bahar ki sites ko nahi dikhana:
```js
fetch('https://another.com/page', { referrerPolicy: "origin-when-cross-origin" });
```
Referrer policy sirf `fetch` ke liye nahi: poore page ke liye `Referrer-Policy` HTTP header, ya link par `<a rel="noreferrer">`.

## `mode`
Galti se hone wali cross-origin requests se bachne ka safeguard:
- `"cors"` (default): cross-origin allowed
- `"same-origin"`: cross-origin **mana**
- `"no-cors"`: sirf safe cross-origin requests

Tab useful jab URL third-party se aaye aur "power off switch" chahiye.

## `credentials`
Cookies aur HTTP-Authorization bhejne ya nahi:
- `"same-origin"` (default): cross-origin par nahi bhejta
- `"include"`: hamesha bhejta hai (cross-origin server se `Access-Control-Allow-Credentials` chahiye)
- `"omit"`: kabhi nahi, same-origin par bhi nahi

## `cache`
Default me standard HTTP caching (`Expires`, `Cache-Control`, `If-Modified-Since`...). Options:
- `"default"`: standard rules
- `"no-store"`: cache poori tarah ignore (agar `If-Modified-Since`, `If-None-Match`, `If-Unmodified-Since`, `If-Match`, `If-Range` header ho to ye default ban jaata hai)
- `"reload"`: cache se result nahi lega, par response se cache bhar dega (agar headers allow karein)
- `"no-cache"`: cached response ho to conditional request, warna normal; cache bharta hai
- `"force-cache"`: cache ka response use karo (stale bhi), na ho to normal request
- `"only-if-cached"`: cache ka use karo (stale bhi), na ho to error. Sirf `mode: "same-origin"` me chalta hai

## `redirect`
- `"follow"` (default): redirects (301, 302...) khud follow
- `"error"`: redirect par error
- `"manual"`: khud sambhalo. Redirect par special response milta hai (`response.type = "opaqueredirect"`, status waghera khaali)

## `integrity`
Response ko pehle se pata **checksum** se verify karo. SHA-256, SHA-384, SHA-512 supported.
```js
fetch('http://site.com/file', { integrity: 'sha256-abcdef' });
```
Fetch khud SHA-256 nikal ke compare karta hai, mismatch par error.

## `keepalive`
Request page ke **baad bhi** chal sake (page chhodne par bhi). Example: `onunload` me statistics bhejna. Normally page unload par saari related requests abort ho jaati hain, `keepalive` unhe background me poora karta hai:
```js
window.onunload = function() {
  fetch('/analytics', { method: 'POST', body: "statistics", keepalive: true });
};
```
Limitations:
- Body limit **64KB** (saari `keepalive` requests ka total). Bahut stats ho to beech beech me regularly bhejte raho.
- Document unload ho gaya to **server ka response handle nahi** kar sakte (analytics me usually koi dikkat nahi).
