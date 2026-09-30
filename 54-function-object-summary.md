# Function Object aur NFE

## Functions objects hote hain
JS me function ek **object** hai jise call bhi kar sakte ho. Isliye usme properties add/remove kar sakte ho.

## Built-in properties

### `name`
Function ka naam. Agar naam nahi diya aur variable me assign kiya, to JS variable ke naam se guess kar leta hai.
```javascript
let sayHi = function () {};
sayHi.name; // "sayHi"
```
Array ke andar bane function ka naam khaali hota hai, kyunki guess karne ka context nahi milta.

### `length`
Function ke **parameters ki ginti**. Rest parameters (`...more`) count nahi hote.
```javascript
function f(a, b, ...rest) {}
f.length; // 2
```

## Custom properties
Function me apni property bhi jod sakte ho.
```javascript
function sayHi() {
  sayHi.counter++;
}
sayHi.counter = 0;
```
**Dhyan:** `sayHi.counter` aur function ke andar `let counter` do alag cheezein hain, unka aapas me koi link nahi.

### Closure vs function property
- **Closure variable:** bahar ka code usse chhu nahi sakta (private).
- **Function property:** bahar se change ho sakta hai (`counter.count = 10`).

Zarurat ke hisaab se chuno.

## Named Function Expression (NFE)
Function expression ko naam dena:
```javascript
let sayHi = function func(who) {
  if (who) console.log("Hello " + who);
  else func("Guest"); // khud ko call kiya
};
```
Do khaas baatein:
1. Function is naam se **andar se khud ko refer** kar sakta hai.
2. Ye naam **bahar se dikhta nahi**.

Ye kaam kyun aata hai? Agar andar `sayHi(...)` likhte aur baad me bahar `sayHi = null` ho jata, to function toot jata. `func` naam hamesha usi function ko point karta hai.

Ye trick **Function Declaration** me nahi chalti.

## Libraries kaise use karti hain
jQuery ka `$` aur lodash ka `_` ek function hain, jinke upar bahut saari helper functions properties ki tarah lagi hoti hain. Isse sirf ek global naam use hota hai.

## Yaad rakho
- Function = callable object.
- `name` aur `length` built-in properties hain.
- Properties aur variables alag hote hain.
- NFE se function safely khud ko call kar sakta hai.
