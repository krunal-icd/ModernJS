# Property Flags aur Descriptors

Object ki property sirf key-value nahi hoti. Value ke saath 3 **flags** bhi hote hain:

| Flag | `true` ho to | `false` ho to |
|---|---|---|
| `writable` | value badal sakte ho | read-only |
| `enumerable` | loops me dikhti hai | loops me nahi dikhti |
| `configurable` | delete kar sakte ho aur flags badal sakte ho | delete/flags change nahi |

Normal tareeke se banayi property me teeno `true` hote hain.

## Flags dekhna
```javascript
let user = { name: "John" };
Object.getOwnPropertyDescriptor(user, "name");
// { value: "John", writable: true, enumerable: true, configurable: true }
```

## Flags badalna / property banana
```javascript
Object.defineProperty(obj, "name", descriptor);
```
- Property pehle se hai to flags update hote hain.
- Nayi property banti hai to jo flag nahi diya wo **`false`** maana jaata hai.

## Non-writable (read-only)
```javascript
Object.defineProperty(user, "name", { writable: false });
user.name = "Pete"; // strict mode me error, warna chupchaap ignore
```

## Non-enumerable
`for..in` aur `Object.keys` me nahi dikhti. Built-in `toString` isi wajah se loop me nahi aata.
```javascript
Object.defineProperty(user, "toString", { enumerable: false });
```

## Non-configurable
- Property delete nahi ho sakti aur flags nahi badal sakte.
- Ye **ek tarfa raasta** hai. Wapas `true` nahi kar sakte.
- Value phir bhi badal sakti hai agar `writable: true` ho.
- Ek hi exception: `writable` ko `true` se `false` kar sakte ho.

Example: `Math.PI` teeno flags `false` ke saath hai, isliye use koi badal nahi sakta.

**Poori tarah "constant" banane ke liye:**
```javascript
Object.defineProperty(user, "name", { writable: false, configurable: false });
```

## Ek saath kai properties
```javascript
Object.defineProperties(user, {
  name: { value: "John", writable: false },
  surname: { value: "Smith", writable: false }
});
```

## Saare descriptors ek saath / flags-wali cloning
```javascript
Object.getOwnPropertyDescriptors(obj);

let clone = Object.defineProperties({}, Object.getOwnPropertyDescriptors(obj));
```
Normal copy (`clone[key] = obj[key]`) flags aur symbol/non-enumerable properties copy nahi karti. Ye tareeka karta hai.

## Poore object ko lock karna
| Method | Kya rokta hai |
|---|---|
| `Object.preventExtensions(obj)` | nayi property jodna |
| `Object.seal(obj)` | jodna aur hatana (`configurable: false`) |
| `Object.freeze(obj)` | jodna, hatana aur badalna (`configurable: false`, `writable: false`) |

Check karne ke liye: `Object.isExtensible`, `Object.isSealed`, `Object.isFrozen`. Ye practice me kam use hote hain.

## Yaad rakho
- Property = value + 3 flags.
- `defineProperty` se flags badalte hain (nayi property me default `false`).
- `configurable: false` wapas nahi hota.
- `freeze` sabse strict hai.
