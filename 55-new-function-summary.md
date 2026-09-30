# `new Function` Syntax

## Ye kya hai?
Function banane ka ek aur (kam use hone wala) tareeka. Isme function **string se** banta hai, run time par.

```javascript
let func = new Function(arg1, arg2, ..., functionBody);
```

Example:
```javascript
let sum = new Function('a', 'b', 'return a + b');
sum(1, 2); // 3

let sayHi = new Function('alert("Hello")');
sayHi();
```

## Kab kaam aata hai?
Jab code pehle se pata nahi hota, jaise server se code string aaye aur use function banakar chalana ho. Ya complex apps me template se function compile karna ho.

## Sabse bada farak: Closure nahi banta
Normal function apne birth-place ke variables yaad rakhta hai. Lekin `new Function` ka `[[Environment]]` **global** ko point karta hai. Matlab wo sirf **global variables** dekh sakta hai, outer function ke local variables nahi.

```javascript
function getFunc() {
  let value = "test";
  let func = new Function('alert(value)');
  return func;
}
getFunc()(); // Error: value defined nahi hai
```
Normal function hota to "test" mil jaata.

## Ye aisa kyun hai?
Production se pehle JS ko **minifier** chhota karta hai aur local variable ke naam badal deta hai (jaise `userName` ko `a`). Agar `new Function` outer variables dekh pata, to string me likha purana naam dhoondhta aur minify ke baad milta hi nahi. Design ke hisaab se bhi ye kharab hota.

## Data kaise dein?
Jo chahiye use **arguments** me pass karo.

## Summary
- `new Function` string se function banata hai.
- Arguments comma se bhi de sakte ho: `new Function('a,b', 'return a+b')`.
- Ye outer variables access nahi kar sakta, sirf global.
- Ye restriction achhi hai: explicit parameters se code safe rehta hai aur minifier ke saath problem nahi aati.
