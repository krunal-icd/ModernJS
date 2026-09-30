# 82. Anchors: `^` (start) aur `$` (end)

Caret `^` aur dollar `$` regexp me special hain, inhe **anchors** kehte hain.
- `^` text ki **shuruaat** par match karta hai
- `$` text ke **end** par match karta hai

```js
/^Mary/.test("Mary had a little lamb");          // true  (Mary se shuru)
/snow$/.test("its fleece was white as snow");    // true  (snow par khatam)
```
Aise simple cases me `startsWith/endsWith` string methods bhi chal jaate hain. Regexps **complex tests** ke liye hain.

## Poora match check karna: `^...$`
Dono anchors saath (`^...$`) aksar yeh check karne ke liye lagte hain ki **poori string** pattern se match karti hai ya nahi (jaise user input ka format).

Time `12:34` format me hai ya nahi (do digits, colon, do digits):
```js
let regexp = /^\d\d:\d\d$/;

regexp.test("12:34");    // true
regexp.test("12:345");   // false
```
Match text ki shuruaat (`^`) ke turant baad shuru ho aur end (`$`) uske turant baad aaye. Zara sa bhi extra character ho to `false`.

Anchors `m` flag ke saath alag behave karte hain (agla chapter).

## Anchors "zero width" hote hain
`^` aur `$` **tests** hain, characters nahi. Ye kisi character ko match nahi karte, balki regexp engine ko condition check karne par majboor karte hain (text ka start/end).

## Task: `^$` kaun si string match karta hai?
Sirf **khaali string** `""`: wo shuru bhi hoti hai aur turant khatam bhi. Ye task phir dikhata hai ki anchors characters nahi, tests hain.
