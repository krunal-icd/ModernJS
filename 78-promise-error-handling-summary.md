# Promises me Error Handling

## Error seedha nazdeeki handler tak jaata hai
Promise reject ho to control **sabse nazdeek wale rejection handler** (`.catch`) par jaata hai. `.catch` turant baad me hona zaroori nahi, kai `.then` ke baad bhi ho sakta hai.

Sabse aasan tareeka: chain ke **aakhir me ek `.catch`** lagao, wo upar ke saare errors pakad leta hai.
```javascript
fetch("user.json")
  .then(r => r.json())
  .then(user => fetch(`https://api.github.com/users/${user.name}`))
  .then(r => r.json())
  .catch(error => console.log(error.message));
```

## Implicit try...catch
Executor aur `.then` handlers ke charon taraf ek **chhupa hua `try...catch`** hota hai. Andar `throw` karo ya galti ho jaaye to wo reject me badal jaata hai.
```javascript
new Promise((resolve, reject) => {
  throw new Error("Whoops!");
}).catch(console.log); // Error: Whoops!
```
Ye `.then` ke andar bhi chalta hai (jaise koi programming error `blabla()`).

## Rethrowing
- `.catch` ne error **handle** kar liya (normal khatam hua) to chain **agle successful `.then`** par chali jaati hai.
- `.catch` ne error handle nahi kiya to `throw error` karo, wo **agle `.catch`** par jaayega.

```javascript
.catch(function (error) {
  if (error instanceof URIError) {
    // handle
  } else {
    throw error; // pata nahi, aage bhejo
  }
})
```

## Unhandled rejections
Agar reject ho aur koi `.catch` na ho, to error "atak" jaata hai. Engine ise **global error** banata hai (console me dikhta hai).

Browser me pakadne ke liye:
```javascript
window.addEventListener("unhandledrejection", function (event) {
  console.log(event.promise); // kaunsa promise
  console.log(event.reason);  // kaunsi error
});
```
Aise errors aksar recover nahi hote, isliye user ko batao aur server ko report bhejo. Node.js me iske apne tareeke hain.

## Task: `setTimeout` ke andar error
```javascript
new Promise(function (resolve, reject) {
  setTimeout(() => { throw new Error("Whoops!"); }, 1000);
}).catch(alert);
```
**`.catch` nahi chalega.** Implicit `try...catch` sirf executor ke chalte waqt wale (synchronous) errors pakadta hai. `setTimeout` ka error baad me hota hai, tab promise use nahi pakad sakta. (`reject(...)` use karna hoga.)

## Yaad rakho
- `.catch` reject aur handler ke error, dono pakadta hai.
- `.catch` wahan lagao jahan error handle karna aata ho, jo pata na ho use rethrow karo.
- `unhandledrejection` se app ko "chupchaap marne" se bachao.
