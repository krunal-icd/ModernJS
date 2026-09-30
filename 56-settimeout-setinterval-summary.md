# setTimeout aur setInterval

Kisi function ko **abhi nahi, baad me** chalana = "scheduling".

| Method | Kaam |
|---|---|
| `setTimeout` | function ko **ek baar** delay ke baad chalata hai |
| `setInterval` | function ko **baar-baar** har interval par chalata hai |

Ye JS language ka part nahi, browser aur Node.js dete hain.

## setTimeout
```javascript
let id = setTimeout(func, delay, arg1, arg2);
```
- `delay` milliseconds me (1000 ms = 1 sec), default 0.
- Extra arguments function ko mil jaate hain.

```javascript
setTimeout(() => console.log("Hi"), 1000);
setTimeout(sayHi, 1000, "Hello", "John");
```

**Galti mat karna:** `setTimeout(sayHi(), 1000)` galat hai. Yahan function turant chal jaata hai. Sahi: `setTimeout(sayHi, 1000)` (bina brackets).

### Cancel karna
```javascript
let id = setTimeout(...);
clearTimeout(id);
```

## setInterval
Syntax same hai. Rokne ke liye `clearInterval(id)`.

```javascript
let id = setInterval(() => console.log("tick"), 2000);
setTimeout(() => clearInterval(id), 5000); // 5 sec baad band
```

## Nested setTimeout (setInterval ka behtar option)
```javascript
setTimeout(function tick() {
  console.log("tick");
  setTimeout(tick, 2000); // agla call yahin se schedule
}, 2000);
```
Fayde:
- Agla delay pichhle result ke hisaab se badal sakte ho (jaise server busy ho to delay double kar do).
- Do calls ke beech **minimum delay pakka** rehta hai.
- `setInterval` me function chalne ka time bhi interval me se kat jaata hai, isliye asli gap kam ho sakta hai.

## Memory ka dhyan
Scheduled function jab tak chal nahi jaata (ya cancel nahi hota), memory me rehta hai, aur uske outer variables bhi. Zarurat na ho to cancel kar do.

## Zero delay: `setTimeout(func, 0)`
Function **current script khatam hone ke baad** jaldi se chalta hai.
```javascript
setTimeout(() => console.log("World"));
console.log("Hello");
// Hello, phir World
```
Browser me nested timers 5 baar ke baad kam se kam 4ms delay ke saath chalte hain (purani wajah se).

## Yaad rakho
- Timers **exact time guarantee nahi** karte. CPU busy ho, tab background me ho, ya battery saver ho to der ho sakti hai.
- `setTimeout` ka function tabhi chalta hai jab baaki code ka kaam ho jaaye. Heavy loop chal raha ho to timer ko wait karna padta hai.
