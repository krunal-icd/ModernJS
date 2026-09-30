# 33. Drag'n'Drop (Mouse Events se)

Browser ke apne native drag events (`dragstart`, `dragend`) hain, par unki limits hain (kisi area se drag rokna, sirf horizontal/vertical drag, mobile support kamzor). Isliye hum mouse events se khud banate hain.

## Basic algorithm
1. `mousedown`: element ko move ke liye taiyar karo (`position:absolute`, `z-index` upar).
2. `mousemove`: `left/top` badlo.
3. `mouseup`: kaam khatam karo, handlers hatao.

```js
ball.onmousedown = function(event) {
  ball.style.position = 'absolute';
  ball.style.zIndex = 1000;
  document.body.append(ball);   // body me daalo taaki body ke hisaab se position ho

  function moveAt(pageX, pageY) {
    ball.style.left = pageX - ball.offsetWidth / 2 + 'px';
    ball.style.top = pageY - ball.offsetHeight / 2 + 'px';
  }
  moveAt(event.pageX, event.pageY);

  function onMouseMove(event) { moveAt(event.pageX, event.pageY); }
  document.addEventListener('mousemove', onMouseMove);

  ball.onmouseup = function() {
    document.removeEventListener('mousemove', onMouseMove);
    ball.onmouseup = null;
  };
};
```

## 2 zaruri baatein
1. **Browser ka native drag band karo**, warna ball "fork" ho jaata hai (clone drag hota hai):
```js
ball.ondragstart = function() { return false; };
```
2. `mousemove` **`document` par** sunte hain, ball par nahi, kyunki tez move me pointer ball se bahar kahin bhi jump kar sakta hai.

## Sahi positioning (shift yaad rakho)
Bina iske, ball ke kinare se pakdo to wo jump karke center me aa jaata hai. Isliye `mousedown` par pointer aur ball ke top-left ka farak yaad rakho:
```js
let shiftX = event.clientX - ball.getBoundingClientRect().left;
let shiftY = event.clientY - ball.getBoundingClientRect().top;

// mousemove me
ball.style.left = event.pageX - shiftX + 'px';
ball.style.top  = event.pageY - shiftY + 'px';
```

## Droppable targets (kis par gira rahe hain?)
Seedhi soch: droppable elements par `mouseover/mouseup` lagao. **Ye kaam nahi karta**, kyunki dragging element hamesha sabse upar hota hai aur events sirf top element par hote hain.

**Solution: `document.elementFromPoint(clientX, clientY)`.** Isse pehle dragging element ko chhupao (`hidden = true`), warna wahi wapas milega:
```js
let currentDroppable = null;

function onMouseMove(event) {
  moveAt(event.pageX, event.pageY);

  ball.hidden = true;
  let elemBelow = document.elementFromPoint(event.clientX, event.clientY);
  ball.hidden = false;

  if (!elemBelow) return;   // window ke bahar to null

  let droppableBelow = elemBelow.closest('.droppable');

  if (currentDroppable != droppableBelow) {
    if (currentDroppable) leaveDroppable(currentDroppable);
    currentDroppable = droppableBelow;
    if (currentDroppable) enterDroppable(currentDroppable);
  }
}
```

## Summary
- Flow: `ball.mousedown` -> `document.mousemove` -> `ball.mouseup` (aur `ondragstart` cancel karo).
- Shift (`shiftX/Y`) yaad rakho.
- Droppable ka pata `elementFromPoint` se lagao.
- Isi base par aur features: `mouseup` par drop finalize, highlight, area/direction limit, aur **event delegation** se hazaaron elements ka drag manage karna.

## Tasks (short)
- **Slider:** sirf horizontal drag, thumb ko slider ki width ke andar rokna (left < 0 ya > right edge par clamp).
- **Superheroes drag karna:** ek `document` handler (delegation), dragging ke waqt `position:fixed` (coordinates aasan), end me `absolute`; window ke top/bottom par pahunche to `window.scrollTo` se page scroll.
