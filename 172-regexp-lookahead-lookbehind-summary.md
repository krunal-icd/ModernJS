# 92. Lookahead aur Lookbehind

Kabhi kabhi sirf wahi matches chahiye jinke **baad** ya **pehle** koi aur pattern ho. Iske liye "**lookahead**" aur "**lookbehind**" (milkar "**lookaround**"). Example: `1 turkey costs 30€` se price (number jiske baad `€` ho).

## Lookahead: `X(?=Y)`
"`X` dhundho, par tabhi match karo jab uske **baad `Y`** ho." `X` aur `Y` koi bhi pattern.
```js
"1 turkey costs 30€".match(/\d+(?=€)/);   // 30  (1 ignore, kyunki uske baad € nahi)
```
Lookahead sirf ek **test** hai. `(?=...)` ke andar ka content result (`30`) me shamil **nahi** hota.

Engine `X` dhundhta hai, phir dekhta hai ki turant baad `Y` hai ya nahi; nahi to potential match skip.

Kai tests: `X(?=Y)(?=Z)` = `X` ke baad **`Y` aur `Z` dono** ek saath (ye tabhi possible jab `Y`, `Z` aapas me exclusive na hon). Jaise `\d+(?=\s)(?=.*30)`: digits jinke baad space ho aur aage kahin `30` bhi ho:
```js
"1 turkey costs 30€".match(/\d+(?=\s)(?=.*30)/);   // 1
```

## Negative lookahead: `X(?!Y)`
"`X` dhundho, par tabhi jab uske baad `Y` **na** ho." Quantity chahiye (price nahi):
```js
"2 turkeys cost 60€".match(/\d+\b(?!€)/g);   // 2  (price nahi mila)
```

## Lookbehind
> Lookbehind non-V8 browsers (Safari, IE) me supported nahi tha.

Lookahead "kya **baad me** aata hai" ki condition deta hai. Lookbehind "kya **pehle** hai" ki.
- **Positive lookbehind:** `(?<=Y)X`: `X` tabhi jab uske **pehle `Y`** ho.
- **Negative lookbehind:** `(?<!Y)X`: `X` tabhi jab uske pehle `Y` **na** ho.

Dollar amount (`$` number ke pehle): `(?<=\$)\d+`
```js
"1 turkey costs $30".match(/(?<=\$)\d+/);   // 30
```
Quantity (number jiske pehle `$` na ho): `(?<!\$)\d+`
```js
"2 turkeys cost $60".match(/(?<!\$)\b\d+/g);   // 2
```

## Capturing groups
Lookaround ke parentheses ka content aam taur par result me nahi aata (jaise `\d+(?=€)` me `€` nahi aata). Par kabhi kabhi use (ya uske hisse ko) bhi capture karna hota hai: **extra parentheses** lagao.
```js
"1 turkey costs 30€".match(/\d+(?=(€|kr))/);   // 30, €   (extra parentheses)
"1 turkey costs $30".match(/(?<=(\$|£))\d+/);  // 30, $
```

## Summary
Lookaround tab kaam aata hai jab kisi cheez ko uske context (pehle/baad) ke hisaab se match karna ho. Simple cases me khud bhi kar sakte hain (sab match karke loop me context filter), par lookaround aam taur par zyaada convenient hai.

| Pattern | Type | Matches |
|---|---|---|
| `X(?=Y)` | Positive lookahead | `X` agar uske baad `Y` |
| `X(?!Y)` | Negative lookahead | `X` agar uske baad `Y` nahi |
| `(?<=Y)X` | Positive lookbehind | `X` agar `Y` ke baad |
| `(?<!Y)X` | Negative lookbehind | `X` agar `Y` ke baad nahi |

## Tasks
**Non-negative integers (`0 12 -5 123 -18` me se `0, 12, 123`):**
`(?<!-)\d+` se `-18` ka `8` bhi mil jaata hai. Fix: `/(?<![-\d])\d+/g` (pehle na `-` ho na koi digit, taaki number beech se shuru na ho).

**`<body>` tag ke turant baad `<h1>Hello</h1>` insert karna (body me attributes ho sakte hain):**
```js
str.replace(/<body.*?>/, '$&<h1>Hello</h1>');        // $& = match khud
str.replace(/(?<=<body.*?>)/, `<h1>Hello</h1>`);     // lookbehind: khaali string ko replace, sirf us position par
```
(`s` aur `i` flags bhi kaam aa sakte hain: newline aur `<BODY>` ke liye.)
