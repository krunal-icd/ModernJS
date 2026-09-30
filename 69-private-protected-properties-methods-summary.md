# Private aur Protected Properties/Methods

## Idea: encapsulation
Coffee machine ko chalane ke liye sirf button dabana aana chahiye, andar ke tar-pipe nahi. Object me bhi **internal interface** (andar ka kaam) ko **external interface** (bahar se use hone wala) se alag rakhte hain.

Fayde:
- User galti se andar ki cheezein tod nahi paata.
- Class likhne wala internals ko freely badal sakta hai, bahar ka code nahi tootta.
- Complexity chhup jaati hai, use karna aasan ho jaata hai.

## Teen type ke fields
| Type | Kaun access kar sakta hai | JS me |
|---|---|---|
| Public | sab | normal |
| Protected | class aur uski child classes | sirf convention (`_naam`) |
| Private | sirf wahi class | language level (`#naam`) |

## Protected: `_` prefix
Ye sirf ek convention hai, JS rokta nahi. "Bahar se mat chhuo."
```javascript
class CoffeeMachine {
  _waterAmount = 0;

  set waterAmount(value) {
    if (value < 0) value = 0;
    this._waterAmount = value;
  }
  get waterAmount() {
    return this._waterAmount;
  }

  constructor(power) { this._power = power; }
}
```
Protected fields child classes me mil jaate hain (inherit hote hain).

## Read-only property
Sirf getter do, setter mat do:
```javascript
get power() { return this._power; }
// coffeeMachine.power = 25;  -> error
```
`getWaterAmount()` / `setWaterAmount()` jaise functions bhi likh sakte ho. Ye zyada flexible hote hain (kai arguments le sakte hain), get/set syntax chhota hota hai. Dono theek hain.

## Private: `#` prefix
Language khud enforce karti hai.
```javascript
class CoffeeMachine {
  #waterLimit = 200;

  #checkWater(value) {
    if (value < 0) return 0;
    if (value > this.#waterLimit) return this.#waterLimit;
    return value;
  }
}
```
- Bahar se `machine.#waterLimit` karoge to **error**.
- **Child class se bhi access nahi** ho sakta.
- Public aur private same naam se ek saath ho sakte hain (`waterAmount` getter + `#waterAmount` field).
- `this["#name"]` jaisa dynamic access private fields par nahi chalta.

Kyunki child class se bhi access nahi hota, kai baar protected (`_`) zyada practical lagta hai.

## Yaad rakho
- Protected = `_`, convention, child classes use kar sakti hain.
- Private = `#`, language enforce karti hai, sirf usi class ke andar.
- Bahar ke liye simple public interface rakho, baaki chhupa do.
