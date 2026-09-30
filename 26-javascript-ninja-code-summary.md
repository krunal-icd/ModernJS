# Ninja Code – Simple Hinglish Summary

Source: https://javascript.info/ninja-code

## Main baat
Ye chapter **sarcasm (vyang)** me likha gaya hai. Isme "ninja programmers" ki wo **bure tricks** batayi gayi hain jinse code padhna aur maintain karna **mushkil** ho jata hai. Asli maqsad ye hai ki tum **pehchano ki kya NAHI karna chahiye.**

Har "advice" ko ulta padho: jo yahan "achha" bataya gaya hai, wo asal me **galat practice** hai.

Author ke shabdon me: ye sab advice **real code** se li gayi hain, kabhi kabhi experienced developers ne bhi likhi hain.

## Ninja tricks (aur unka sahi ulta)

### 1. Code ko bahut chhota likho ("Brevity")
**Ninja way:** Code jitna ho sake chhota likho, language ke subtle features use karke apni hoshiyari dikhao.

```javascript
// ek famous library se
i = i ? i < 0 ? Math.max(0, len + i) : i : 0;
```

**Problem:** Dusra developer is line ko samajhne me bahut time lagayega.
**Sahi tarika:** Code **readable** likho, chhota nahi.

### 2. Ek-akshar wale variables
**Ninja way:** Har jagah `a`, `b`, `c` jaise naam use karo. Loop counter me `i` ki jagah `x` ya `y` use karo, khaaskar jab loop body 1-2 page lambi ho.

**Problem:** Editor ke search me nahi milta, aur naam se kuch pata nahi chalta.
**Sahi tarika:** **Descriptive naam** rakho.

### 3. Abbreviations
**Ninja way:** `list` → `lst`, `userAgent` → `ua`, `browser` → `brsr`.

**Sahi tarika:** Naam ko chhota mat karo, poora aur saaf likho.

### 4. Bahut abstract naam
**Ninja way:**
- Sabse ideal naam `data` hai, use har jagah lagao. `data` le liya to `value` use karo.
- Variable ko uske **type** ke naam se bulao: `str`, `num`.
- Naam khatam ho jaye to number jodo: `data1`, `item2`, `elem5`.

**Problem:** Naam sirf ye batata hai ki andar kya type hai, ye nahi ki **kya cheez** hai.
**Sahi tarika:** Naam batae ki variable **kya store karta hai** (jaise `userName`, `shoppingCart`).

### 5. Attention test (milte-julte naam)
**Ninja way:** `date` aur `data` jaise milte-julte naam mix karo.

**Problem:** Jaldi padhna namumkin, aur typo hone par bahut der tak atke rahoge.

### 6. Smart synonyms
**Ninja way:** Ek hi tarah ke kaam ke liye alag alag prefixes use karo (`displayMessage`, `showName`, `renderTitle`, `paintText`). Aur **alag kaam** wale functions ke liye **same prefix** use karo: `printPage` (printer par), `printText` (screen par), `printMessage` (naye window me).

**Problem:** Padhne wala confuse ho jata hai ki kaunsa function kya karta hai.
**Sahi tarika:** Ek hi kaam ke liye **ek hi prefix**, aur alag kaam ke liye alag.

### 7. Naam reuse karo
**Ninja way:** Naya variable tabhi banao jab bilkul zaruri ho. Purane me hi nayi values likhte raho. Function ke beech me value ko chupke se replace kar do:

```javascript
function ninjaFunction(elem) {
  // 20 lines of code working with elem

  elem = clone(elem);

  // 20 more lines, ab clone par kaam ho raha hai!
}
```

**Problem:** Pata hi nahi chalta ki variable me abhi kya hai.
**Sahi tarika:** **Alag values ke liye alag variables.** Extra variable achha hota hai, bura nahi.

### 8. Underscores mazaak ke liye
**Ninja way:** Variables ke pehle `_` ya `__` lagao, bina kisi matlab ke, ya alag jagah alag matlab ke saath.

**Problem:** Code lamba aur kam readable ho jata hai.

### 9. "Super", "mega", "nice" jaise naam
**Ninja way:** `superElement`, `megaFrame`, `niceItem` jaise naam rakho.

**Problem:** Ye kuch details nahi dete. Padhne wala matlab dhundhta rehta hai.

### 10. Bahar ke variable ko overlap karo
**Ninja way:** Function ke andar aur bahar same naam use karo:

```javascript
let user = authenticateUser();

function render() {
  let user = anotherValue();
  ...
  ...many lines...
  ... // <-- programmer yahan user use karna chahta hai...
}
```

**Problem:** Programmer ko pata hi nahi chalta ki local `user` ne bahar wale ko **shadow** kar diya hai.

### 11. Side-effects har jagah
**Ninja way:** `isReady()`, `checkPermission()`, `findTags()` jaise functions me "useful" extra action daal do jo bahar ka kuch badal de. Ya `true/false` ki jagah ek complex object return karo.

**Problem:** Sab maante hain ki `is...`, `check...`, `find...` functions kuch badalte nahi. Aise me `if (checkPermission(..))` kaam nahi karega.
**Sahi tarika:** Function jo naam kehta hai wahi kare, usse zyada nahi.

### 12. Powerful functions
**Ninja way:** Function ko naam se zyada kaam karne do. Jaise `validateEmail(email)` email check karne ke saath error message bhi dikhaye aur dobara email maange.

**Problem:** Kisi ko sirf email check karna ho to ye function uske kaam ka nahi, kyunki ye **reuse se bachata hai.**
**Sahi tarika:** **Ek function, ek kaam.**

## Summary
Ye saari "advices" asli code se aayi hain. Inhe follow karoge to:
- Kuch follow karo to code **surprises se bhar jayega.**
- Bahut saari follow karo to code **sirf tumhara** ban jayega, koi badalna nahi chahega.
- Sab follow karo to code **naye developers ke liye ek seekh** ban jayega.

## Quick Table: Ninja vs Sahi tarika

| Ninja trick (galat) | Sahi tarika |
|---------------------|-------------|
| Bahut chhota, "smart" code | Readable code |
| `a`, `b`, `x` jaise naam | Descriptive naam |
| `lst`, `ua`, `brsr` | Poore aur saaf naam |
| `data`, `value`, `str` | Batao variable **kya** store karta hai |
| `date` aur `data` mix | Milte-julte naam mat rakho |
| Same kaam, alag prefixes | Same kaam, same prefix |
| Purane variable me nayi value | Naya variable banao |
| Bekaar `_`, `__` | Sirf jahan matlab ho |
| `superX`, `megaY` | Specific naam |
| Andar-bahar same naam | Alag naam (shadowing se bacho) |
| `check...` me side-effect | Function sirf naam ka kaam kare |
| Ek function me kai kaam | Ek function, ek kaam |
