# 85. Escaping aur Special Characters

Backslash `\` character classes ke liye use hota hai (jaise `\d`), yaani ye regexp me special character hai (strings ki tarah). Aur bhi special characters hain jinka regexp me khaas matlab hai:

**`[ ] { } ( ) \ ^ $ . | ? * +`**

Ye list yaad karne ki zarurat nahi, aage har ek ko seekhenge to apne aap yaad ho jaayegi.

## Escaping
Maano hume literally ek **dot** dhundhna hai, "koi bhi character" nahi. Special character ko normal ki tarah use karne ke liye uske aage backslash lagao: `\.`. Ise "escaping" kehte hain.
```js
"Chapter 5.1".match(/\d\.\d/);   // 5.1 (match!)
"Chapter 511".match(/\d\.\d/);   // null (asli dot chahiye)
```
Parentheses bhi special hain, unhe chahiye to `\(`:
```js
"function g()".match(/g\(\)/);   // "g()"
```
Backslash khud dhundhna ho to wo strings aur regexps dono me special hai, isliye **double** karo:
```js
"1\\2".match(/\\/);   // '\'
```

## Slash
Slash `/` khud special character nahi hai, par JS me `/.../` regexp ko khol-band karta hai, isliye use bhi escape karna padta hai:
```js
"/".match(/\//);   // '/'
```
Par agar `/.../` nahi, `new RegExp` se banao to escape ki zarurat nahi:
```js
"/".match(new RegExp("/"));
```

## `new RegExp` ke saath
`new RegExp` me `/` escape nahi karna, par kuch aur escaping karni padti hai. Ye kaam nahi karta:
```js
let regexp = new RegExp("\d\.\d");
"Chapter 5.1".match(regexp);   // null
```
`/\d\.\d/` chalta tha to ye kyun nahi? Kyunki **strings backslashes ko "kha" jaati hain.** Regular strings ke apne special characters hain (`\n`, `\u1234`), aur jab koi khaas matlab nahi (jaise `\d` ya `\z`) to backslash bas hata diya jaata hai.
```js
alert("\d\.\d");   // d.d
```
To `new RegExp` ko backslash ke bina string milti hai. Fix: backslashes **double** karo (string quotes `\\` ko `\` bana dete hain):
```js
let regStr = "\\d\\.\\d";
alert(regStr);   // \d\.\d (ab sahi)

let regexp = new RegExp(regStr);
"Chapter 5.1".match(regexp);   // 5.1
```

## Summary
- Special characters `[ \ ^ $ . | ? * + ( )` ko literally dhundhna ho to aage `\` lagao ("escape").
- `/.../` ke andar `/` bhi escape karo (`new RegExp` ke andar nahi).
- `new RegExp` ko string dete waqt backslashes **double** karo `\\`, kyunki string quotes ek backslash kha jaate hain.
