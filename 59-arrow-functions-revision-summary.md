# Arrow Functions (dobara)

Arrow function sirf chhota syntax nahi hai. Iske kuch khaas features hain. Ye khaaskar tab kaam aate hain jab koi chhota function kahin aur chalana ho (`forEach`, `setTimeout`) aur hum current context me hi rehna chahte hon.

## 1. Arrow function ka apna `this` nahi hota
`this` use karo to wo **bahar (outer) se** liya jaata hai, normal variable ki tarah.

```javascript
let group = {
  title: "Our Group",
  students: ["John", "Pete"],

  showList() {
    this.students.forEach(
      student => console.log(this.title + ": " + student)
    ); // this = group, sahi chalega
  }
};
```
Agar yahan normal `function(student) {...}` hota, to andar `this = undefined` hota aur error aata.

**Arrow vs `bind`:**
- `bind(this)` naya bound function banata hai.
- Arrow ne kuch bind hi nahi kiya. Uske paas `this` hai hi nahi, isliye bahar se dhoondhta hai.

## 2. Arrow function ka apna `arguments` nahi hota
Ye decorators me kaam aata hai.

```javascript
function defer(f, ms) {
  return function () {
    setTimeout(() => f.apply(this, arguments), ms);
  };
}
```
Yahan arrow ke andar `this` aur `arguments` bahar wale wrapper ke hi milte hain. Normal function se karte to `ctx` aur `args` jaise extra variables banane padte.

## 3. `new` se call nahi ho sakta
`this` nahi hai, to arrow function **constructor** ki tarah use nahi ho sakta.

## 4. `super` bhi nahi hota
(Ye class inheritance me aayega.)

## Summary
Arrow function me nahi hote:
- `this`
- `arguments`
- `new` ke saath use
- `super`

## Kab use karein?
Chhote callbacks ke liye, jahan apna alag context nahi chahiye, balki maujuda context hi use karna hai.
