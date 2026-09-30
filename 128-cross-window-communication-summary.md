# 48. Cross-window Communication

**Same Origin policy** windows aur frames ko ek doosre ka content access karne se rokti hai (taaki `john-smith.com` ki script `gmail.com` ki mail na padh sake).

## Same Origin
Do URLs ka origin same tab hota hai jab **protocol, domain aur port** teeno same hon.

Same:
- `http://site.com`
- `http://site.com/`
- `http://site.com/my/page.html`

Alag:
- `http://www.site.com` (`www.` alag domain)
- `http://site.org`
- `https://site.com` (https)
- `http://site.com:8080` (port)

Policy:
- **Same origin** ho to poora access.
- **Alag origin** ho to variables, document, kuch nahi padh sakte. Sirf **`location` badal** sakte hain (redirect), par **padh nahi** sakte.

## iframe me
- `iframe.contentWindow`: iframe ke andar ki window
- `iframe.contentDocument`: iframe ka document (`contentWindow.document` ka shortcut)

Alag origin ke iframe me:
- `contentWindow` reference lena OK.
- `contentDocument` padhna **ERROR**.
- `location.href` padhna **ERROR**.
- `location` me **likhna OK**.

Same origin me sab kuch allowed. `iframe.onload` (tag par) aur `contentWindow.onload` essentially same hain, par alag origin me `contentWindow.onload` access nahi hota, isliye `iframe.onload` use karo.

## Subdomains: `document.domain`
`john.site.com`, `peter.site.com`, `site.com` ka second-level domain same hai. Har window me:
```js
document.domain = 'site.com';
```
to browser unhe "same origin" maanega. **Deprecated** hai (spec se hataya ja raha hai) par browsers abhi bhi support karte hain. Replacement: cross-window messaging.

## iframe ka "wrong document" pitfall
iframe banate hi uska ek document hota hai, par wo **us document se alag** hota hai jo baad me load hota hai.
```js
let oldDoc = iframe.contentDocument;
iframe.onload = function() {
  alert(oldDoc == iframe.contentDocument);   // false
};
```
Load se pehle document par kuch (handlers) lagaoge to wo **kho jaayega**. Sahi document `iframe.onload` par pakka hota hai. Pehle chahiye to `setInterval` se check karo ki `contentDocument` badla ya nahi.

## `window.frames`
iframe ki window lene ka doosra tarika:
- `window.frames[0]`: pehle frame ki window
- `window.frames.iframeName`: `name="iframeName"` wale frame ki window

Navigation links:
- `window.frames`: children windows
- `window.parent`: parent (bahar wali) window
- `window.top`: sabse upar wali parent window

```js
if (window == top) {
  alert('Topmost window hoon');
} else {
  alert('Frame ke andar hoon');
}
```

## iframe ka `sandbox` attribute
Untrusted code chalne se rokne ke liye. `<iframe sandbox src="...">` = strictest restrictions (iframe ko **alag origin** jaisa treat karta hai). Khaali `sandbox` se sab restrictions lag jaati hain, aur jo hatani ho unhe list karo:
- `allow-same-origin`: "alag origin" wali forcing hatao
- `allow-top-navigation`: iframe `parent.location` badal sake
- `allow-forms`: forms submit
- `allow-scripts`: scripts chalne do
- `allow-popups`: `window.open`

```html
<iframe sandbox="allow-forms allow-popups" src="..."></iframe>
```
`sandbox` sirf **aur restrictions jodta hai**, hata nahi sakta (jaise alag origin ki iframe ki same-origin restriction).

## Cross-window messaging: `postMessage`
Same Origin policy ka safe tareeqa jisse **kisi bhi origin** ki windows aapas me baat kar sakti hain (dono ki sahmati se).

### Bhejna: `postMessage`
```js
win.postMessage(data, targetOrigin);
```
- `data`: koi bhi object (structured cloning). (IE sirf strings, isliye `JSON.stringify`.)
- `targetOrigin`: sirf isi origin ki window ko message milega. Ye safety ke liye hai (user navigate ho gaya ho to sender ko pata nahi chalta, aur sensitive data galat site ko na jaye). Check nahi chahiye to `"*"`.
```js
win.postMessage("message", "http://example.com");
win.postMessage("message", "*");
```

### Sunna: `message` event
Receiver me **`addEventListener`** (`window.onmessage` nahi chalta):
```js
window.addEventListener("message", function(event) {
  if (event.origin != 'http://javascript.info') return;   // anjaan source ignore

  alert("received: " + event.data);
  // event.source.postMessage(...) se jawab bhej sakte ho
});
```
Event ki properties:
- `data`: bheja gaya data
- `origin`: sender ka origin
- `source`: sender window ka reference

Hamesha `event.origin` check karo.

## Summary
- Reference lena: popups (`window.open`, `window.opener`), iframes (`window.frames`, `window.parent`, `window.top`, `iframe.contentWindow`).
- Same origin: sab kuch. Alag origin: sirf `location` badalna aur `postMessage`.
- Exceptions: same second-level domain (`document.domain`), aur `sandbox` (forcefully alag origin jab tak `allow-same-origin` na ho).
- `postMessage`: sender `targetWin.postMessage(data, targetOrigin)`, receiver `message` event (`origin`, `source`, `data`).
