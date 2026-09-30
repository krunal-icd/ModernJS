# 27. Bubbling aur Capturing

## Bubbling
Jab kisi element par event hota hai, **pehle usi ke handlers chalte hain, phir uske parent ke, phir upar tak sab ancestors ke.**
```html
<form onclick="alert('form')">FORM
  <div onclick="alert('div')">DIV
    <p onclick="alert('p')">P</p>
  </div>
</form>
```
`<p>` par click: `p` -> `div` -> `form` (upar `document` tak).

Ise "bubbling" isliye kehte hain ki event paani ke bubble ki tarah andar se upar uthta hai.

**Lagbhag** sabhi events bubble karte hain. Exception: `focus` bubble nahi karta.

## `event.target` vs `this` (`event.currentTarget`)
- `event.target`: sabse **andar wala** element jisne event shuru kiya. Bubbling ke dauran **badalta nahi**.
- `this` / `event.currentTarget`: jis element ka handler abhi chal raha hai.

Agar sirf `form.onclick` hai, to form ke andar kahin bhi click ho, handler chalta hai, `target` asli clicked element hoga aur `this` form.

## Bubbling rokna
```js
event.stopPropagation();          // upar jaane se rok do (isi element ke baaki handlers chalte hain)
event.stopImmediatePropagation(); // isi element ke baaki handlers bhi ruk jaate hain
```
**Bina zarurat ke mat roko!** Analytics jaisa code jo `document` par click sunta hai, wo "dead zone" me kaam nahi karega. Alternative: custom events ya `event` object me data likh do.

## Capturing
Event ke 3 phases hote hain:
1. **Capturing**: `window` -> ... -> parent tak neeche jaata hai
2. **Target**: target element par pahunchta hai
3. **Bubbling**: wapas upar jaata hai

`on<event>`, HTML attribute aur do-argument `addEventListener` **sirf phase 2 aur 3** dekhte hain. Capturing phase pakadne ke liye:
```js
elem.addEventListener("click", handler, true);
elem.addEventListener("click", handler, {capture: true});   // same
```
- `capture: false` (default) = bubbling phase
- `capture: true` = capturing phase

Example order (`<p>` par click): capturing me `HTML -> BODY -> FORM -> DIV -> P`, phir bubbling me `P -> DIV -> FORM -> BODY -> HTML`.

Aur baatein:
- `removeEventListener` me **wahi phase** dena padta hai.
- Ek hi element aur phase par handlers unke lagaye gaye **order** me chalte hain.
- Capturing me `stopPropagation()` chalaya to **bubbling bhi** nahi hoti.
- `event.eventPhase`: 1 = capturing, 2 = target, 3 = bubbling (kam use).

## Kyun zyaadatar bubbling hi use hoti hai?
Jaise real duniya me pehle local authorities react karti hain jo area ko sabse achhe se jaanti hain, phir upar ki. Isi tarah specific element ka handler pehle chalta hai (usko sabse zyada details pata hain), phir parents.

## Summary
- Event: capturing (neeche) -> target -> bubbling (upar).
- `event.target` (asli source), `event.currentTarget` (abhi handler wala).
- `stopPropagation()` avoid karo.
- Bubbling hi **event delegation** ki buniyaad hai (agla topic).
