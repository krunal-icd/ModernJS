# 34. Pointer Events

Mouse, pen/stylus aur touchscreen, sabke input ko ek hi tarah handle karne ka modern tarika.

## Thoda itihaas
1. Pehle sirf **mouse events**. Touch devices par compatibility ke liye mouse events emulate hote the.
2. Touch me multi-touch jaisi cheezein hain, isliye **touch events** (`touchstart`...) aaye. Par pen jaise devices aur "mouse + touch dono ka code" likhna jhanjhat tha.
3. Isliye **Pointer Events** aaye: sab devices ke liye ek set. Ab lagbhag sab browsers support karte hain, to mouse/touch events ki jagah ye use karo.

## Event types (mouse jaise naam)
| Pointer | Mouse |
|---|---|
| `pointerdown` | `mousedown` |
| `pointerup` | `mouseup` |
| `pointermove` | `mousemove` |
| `pointerover / pointerout` | `mouseover / mouseout` |
| `pointerenter / pointerleave` | `mouseenter / mouseleave` |
| `pointercancel` | (koi nahi) |
| `gotpointercapture / lostpointercapture` | (koi nahi) |

Code me `mouse<event>` ko `pointer<event>` se badal do, mouse ke liye sab chalta rahega, touch ka support bhi behtar. (Kabhi kabhi CSS me `touch-action: none` chahiye.)

## Extra properties
- `pointerId`: har pointer (jaise har ungli) ka unique ID
- `pointerType`: `"mouse"`, `"pen"` ya `"touch"`
- `isPrimary`: primary pointer (pehli ungli) ke liye `true`
- `width`, `height`: touch ka contact area (mouse ke liye 1)
- `pressure` (0 se 1), `tangentialPressure`, `tiltX`, `tiltY`, `twist` (pen ke liye)

## Multi-touch
- Pehli ungli: `pointerdown` with `isPrimary = true`.
- Doosri, teesri...: `pointerdown` with `isPrimary = false`, **alag `pointerId`** har ungli ka.
- Move/up par bhi wahi `pointerId`, isse har ungli track hoti hai.
- Mouse ke liye hamesha same `pointerId` aur `isPrimary = true`.

## `pointercancel`
Jab koi pointer interaction beech me abort ho jaye:
- device band ho gaya, orientation badla,
- **browser ne khud interaction sambhal liya** (jaise image ka native drag, zoom/pan).

Drag'n'drop me browser image ka native drag shuru kar deta hai aur `pointercancel` fire hota hai (phir `pointermove` nahi aate). Fix:
1. `ball.ondragstart = () => false;`
2. Touch ke liye CSS me `#ball { touch-action: none }`

## Pointer capturing
`elem.setPointerCapture(pointerId)`: is `pointerId` ke **saare aage ke events `elem` par retarget** ho jaate hain (chahe pointer kahin bhi ho).

Hataya jaata hai:
- `pointerup` / `pointercancel` par apne aap
- `elem` document se hate to
- `elem.releasePointerCapture(pointerId)` se

Slider ka example:
```js
thumb.onpointerdown = function(event) {
  thumb.setPointerCapture(event.pointerId);

  thumb.onpointermove = function(event) {
    let newLeft = event.clientX - slider.getBoundingClientRect().left;
    thumb.style.left = newLeft + 'px';
  };

  thumb.onpointerup = function() {
    thumb.onpointermove = null;
    thumb.onpointerup = null;
  };
};
```
Fayde:
1. `document` par handlers lagane/hatane nahi padte.
2. Drag ke dauran dusre elements ke handlers (jaise `mouseover`) galti se nahi chalte.

Coordinates (`clientX/Y`) sahi hi rehte hain, capture sirf `target/currentTarget` badalta hai.

Capture ke events: `gotpointercapture` (capture shuru), `lostpointercapture` (capture khatam).

## Summary
- Pointer events = mouse + touch + pen ek code me.
- Multi-touch: `pointerId` + `isPrimary`.
- Drag/touch interactions me default action roko aur `touch-action: none` lagao.
- `setPointerCapture` drag ko saaf banata hai.
