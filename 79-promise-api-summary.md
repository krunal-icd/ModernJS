# Promise API

`Promise` class ke 6 static methods hain.

## 1. `Promise.all`
Kai promises **parallel** chalao aur **sabke poore hone** ka wait karo.
```javascript
Promise.all([p1, p2, p3]).then(results => ...);
```
- Result ek array hota hai, aur order **wahi rehta hai jo input me tha** (chahe koi pehle khatam ho).
- Trick: data ko `map` karke promises ki array banao.
```javascript
let requests = urls.map(url => fetch(url));
Promise.all(requests).then(responses => ...);
```
- **Koi ek bhi reject** hua to `Promise.all` turant reject ho jaata hai, baaki ke results ignore ho jaate hain (par wo requests cancel nahi hoti).
- Array me normal values (promise nahi) bhi de sakte ho, wo jaise hain waise result me aa jaati hain.

## 2. `Promise.allSettled`
Sab ke **settle hone** ka wait karta hai, chahe success ho ya fail. Result me har ek ka status milta hai:
- `{ status: "fulfilled", value }`
- `{ status: "rejected", reason }`

Kaam ka: jab kuch requests fail ho jaayein tab bhi baaki chahiye ho.
```javascript
Promise.allSettled(urls.map(u => fetch(u))).then(results => {
  results.forEach((r, i) => {
    if (r.status === "fulfilled") console.log(urls[i], r.value.status);
    else console.log(urls[i], r.reason);
  });
});
```
(Purane browsers ke liye polyfill bana sakte ho.)

## 3. `Promise.race`
**Sabse pehle settle** hone wale promise ka result (ya error) leta hai. Baaki ignore.
```javascript
Promise.race([slowP, fastP]).then(...); // fastP ka result
```

## 4. `Promise.any`
**Sabse pehle fulfill** hone wale promise ka result leta hai. Reject hone wale promises ko skip karta hai. Agar **saare reject** ho jaayein to `AggregateError` deta hai, jiske `errors` property me saare errors hote hain.

## 5. `Promise.resolve(value)`
Ready value se resolved promise banata hai. Jab function ko hamesha promise return karna ho (jaise cache se turant value dena):
```javascript
function loadCached(url) {
  if (cache.has(url)) return Promise.resolve(cache.get(url));
  return fetch(url).then(r => r.text()).then(text => { cache.set(url, text); return text; });
}
```

## 6. `Promise.reject(error)`
Rejected promise banata hai. Practice me bahut kam use hota hai. (Aajkal `async/await` ki wajah se `resolve/reject` methods ki zarurat kam padti hai.)

## Summary table
| Method | Kab khatam hota hai | Result |
|---|---|---|
| `all` | sab fulfill hon | results ki array (ek fail to poora fail) |
| `allSettled` | sab settle hon | status objects ki array |
| `race` | pehla settle | uska result/error |
| `any` | pehla fulfill | uska result (sab fail to `AggregateError`) |
| `resolve` | turant | value ke saath resolved |
| `reject` | turant | error ke saath rejected |

Inme se `Promise.all` sabse zyada use hota hai.
