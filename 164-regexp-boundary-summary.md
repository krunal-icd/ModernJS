# 84. Word boundary: `\b`

`\b` bhi `^` aur `$` ki tarah ek **test** hai. Regexp engine `\b` par pahunchta hai to check karta hai ki string me wo position "word boundary" hai ya nahi.

Teen positions word boundary maani jaati hain:
1. String ki **shuruaat**, agar pehla character word character `\w` ho.
2. String ke **beech** me do characters ke beech jahan ek `\w` ho aur doosra nahi.
3. String ke **end** par, agar aakhri character `\w` ho.

Jaise `\bJava\b` `Hello, Java!` me mil jaata hai (standalone word), par `Hello, JavaScript!` me nahi:
```js
"Hello, Java!".match(/\bJava\b/);         // Java
"Hello, JavaScript!".match(/\bJava\b/);   // null
```

`Hello, Java!` me `\b` in positions par hai: shuruaat (H se pehle), `Hello` ke baad (o aur comma ke beech), `Java` se pehle (space aur J ke beech) aur `Java` ke baad (a aur `!` ke beech).

To `\bHello\b` match hoga, par `\bHell\b` nahi (`l` ke baad boundary nahi) aur `Java!\b` bhi nahi (`!` word character nahi hai, isliye uske baad boundary nahi):
```js
"Hello, Java!".match(/\bHello\b/);   // Hello
"Hello, Java!".match(/\bJava\b/);    // Java
"Hello, Java!".match(/\bHell\b/);    // null
"Hello, Java!".match(/\bJava!\b/);   // null
```

## Digits ke saath
`\b` sirf words nahi, digits ke saath bhi. `\b\d\d\b` **akele 2-digit numbers** dhundhta hai (jinke aas-paas `\w` na ho, jaise space, punctuation ya text ka start/end):
```js
"1 23 456 78".match(/\b\d\d\b/g);   // 23,78
"12,34,56".match(/\b\d\d\b/g);      // 12,34,56
```

## Non-Latin alphabets ke liye `\b` kaam nahi karta
`\b` check karta hai ki position ke ek taraf `\w` ho aur doosri taraf `\w` na ho. Par `\w` matlab Latin letters `a-z`, digits, underscore. To Cyrillic letters ya hieroglyphs ke liye ye test kaam nahi karta.

## Task: time dhundhna
Format `hours:minutes` (jaise `09:00`), string `Breakfast at 09:00 in the room 123:456.` me se, par `123:456` match **na** ho:
```js
/\b\d\d:\d\d\b/    // 09:00
```
(Sahi time hai ya nahi ye check nahi karna, `25:99` bhi chalega abhi.)
