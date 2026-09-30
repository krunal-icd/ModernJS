# 83. Multiline mode: `m` flag (`^` aur `$` ke liye)

`m` flag sirf **`^` aur `$` ka behavior** badalta hai. Multiline mode me ye sirf string ke start/end par nahi, balki **har line ke start/end** par match karte hain.

## Line ke start par `^`
`/^\d/gm` har line ki shuruaat ka digit leta hai:
```js
let str = `1st place: Winnie
2nd place: Piglet
3rd place: Eeyore`;

str.match(/^\d/gm);   // 1, 2, 3
str.match(/^\d/g);    // 1   (bina "m" ke sirf pehla)
```
By default `^` sirf text ki shuruaat par match karta hai, multiline mode me **kisi bhi line ki shuruaat** par.

"Line ki shuruaat" ka formal matlab: "line break ke **turant baad**". Multiline mode me `^` un sab positions par match karta hai jinse pehle newline `\n` ho, aur text ki shuruaat par bhi.

## Line ke end par `$`
`$` bhi aise hi. `\d$` har line ka aakhri digit dhundhta hai:
```js
let str = `Winnie: 1
Piglet: 2
Eeyore: 3`;

str.match(/\d$/gm);   // 1,2,3
```
Bina `m` ke `$` sirf poore text ke end par match karta, to sirf aakhri digit milta.

"Line ka end" ka matlab "line break se **turant pehle**", aur text ke end par bhi.

## `^ $` ke bajay `\n` dhundhna
Newline dhundhne ke liye anchors ke alawa `\n` character bhi use kar sakte hain. Farak dekho, `\d$` ke bajay `\d\n`:
```js
str.match(/\d\n/g);   // 1\n,2\n
```
Yaha **2 matches** milte hain, 3 nahi, kyunki `3` ke baad newline hai hi nahi (text ka end hai, jo `$` se match hota).

Doosra farak: har match me ab `\n` character shamil hai. Anchors sirf condition test karte hain, par `\n` ek **character** hai jo result ka hissa ban jaata hai.

Yaani:
- Result me newline chahiye to pattern me `\n`
- Line ki shuruaat/end par kuch dhundhna ho to anchors (`^`, `$`) with `m` flag
