# Closure aur Variable Scope

## Block scope (let/const)
`{ }` ke andar `let` ya `const` se banaya variable sirf usi block me dikhta hai. `if`, `for`, `while` ke andar bhi yahi rule hai.

```javascript
if (true) {
  let name = "Raj";
}
console.log(name); // Error: name defined nahi hai
```

## Nested function
Function ke andar function banana nested function kehlata hai. Andar wala function bahar ke (outer) variables use kar sakta hai.

## Lexical Environment (simple bhasha me)
- Har function, block aur poori script ke paas ek hidden "dabba" hota hai jisme uske variables rakhe jaate hain.
- Is dabbe ke paas ek **link** bhi hota hai apne bahar wale dabbe ka.
- Variable dhoondhte waqt JS pehle apna dabba dekhta hai, phir bahar wala, phir usse bahar wala... global tak.

## Closure kya hai?
**Closure = aisa function jo yaad rakhta hai ki wo kahan bana tha, aur wahan ke variables use kar sakta hai, chahe usko kahin se bhi call karo.**

JS me saare functions automatically closure hote hain (sirf `new Function` exception hai).

```javascript
function makeCounter() {
  let count = 0;
  return function () {
    return count++;
  };
}

let c1 = makeCounter();
let c2 = makeCounter();

c1(); // 0
c1(); // 1
c2(); // 0  -> c2 ka apna alag count hai
```

Har `makeCounter()` call par naya dabba banta hai, isliye counters ek dusre se independent hain.

## Zaroori baatein
- Function ko variable ki **latest value** milti hai, purani copy nahi.
- `let` variable declare hone se pehle use karoge to error aata hai. Is beech ke time ko **"dead zone"** kehte hain.
- **Garbage collection:** jab tak koi inner function zinda hai, uska outer dabba memory me rehta hai. Function `null` kar do to memory free ho jaati hai.
- `for (let i...)` me har iteration ka apna alag `i` hota hai. Isliye loop me functions banate waqt `let` ke saath `for` safe rehta hai.

## Yaad rakho
1. Function apne birth-place ke variables yaad rakhta hai (closure).
2. Variable wahi update hota hai jahan wo rehta hai.
3. Alag-alag calls ke alag-alag dabbe hote hain.
