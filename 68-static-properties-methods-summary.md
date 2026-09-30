# Static Properties aur Methods

## Static method kya hai?
Wo method jo **poori class** ka hota hai, kisi ek object ka nahi. `static` keyword lagate hain.

```javascript
class User {
  static staticMethod() {
    console.log(this === User); // true
  }
}
User.staticMethod();
```
Ye `User.staticMethod = function...` likhne jaisa hi hai. Andar `this` = class khud (dot se pehle wala rule).

## Kab use hota hai?
1. **Compare/helper functions:**
```javascript
class Article {
  constructor(title, date) { this.title = title; this.date = date; }
  static compare(a, b) { return a.date - b.date; }
}
articles.sort(Article.compare);
```
2. **Factory methods** (alag tareeke se object banane ke liye):
```javascript
static createTodays() {
  return new this("Today's digest", new Date());
}
```
3. Database jaisi classes me search/save/remove.

**Dhyan:** static methods sirf class par chalte hain, object par nahi.
```javascript
article.createTodays(); // Error
```

## Static properties
```javascript
class Article {
  static publisher = "Ilya Kantor";
}
Article.publisher;
```
Class-level data ke liye (kisi instance se nahi jura).

## Static ki inheritance
Static properties/methods **inherit** hote hain.
```javascript
class Animal {
  static planet = "Earth";
  static compare(a, b) { return a.speed - b.speed; }
}
class Rabbit extends Animal {}

Rabbit.planet;   // Earth
Rabbit.compare;  // Animal.compare
```
Kaise? `extends` **do** prototype links banata hai:
1. `Rabbit.__proto__ === Animal` (statics ke liye)
2. `Rabbit.prototype.__proto__ === Animal.prototype` (normal methods ke liye)

## Task: `class Rabbit extends Object`
| `class Rabbit` | `class Rabbit extends Object` |
|---|---|
| kuch nahi | constructor me `super()` zaroori |
| `Rabbit.__proto__ === Function.prototype` | `Rabbit.__proto__ === Object` |

Matlab `extends Object` karne par `Rabbit.getOwnPropertyNames(...)` jaise `Object` ke static methods bhi mil jaate hain.

## Yaad rakho
- `static` = class ka apna kaam/data.
- Object par nahi, class par call karo.
- Child class ko statics parent se mil jaate hain.
