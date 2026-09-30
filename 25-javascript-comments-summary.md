# Comments – Simple Hinglish Summary

Source: https://javascript.info/comments

## Main baat
Comments do tarah ke hote hain: **single-line** (`//`) aur **multiline** (`/* ... */`). Hum inhe aam taur par ye batane ke liye use karte hain ki **code kaise aur kyu kaam karta hai.**

Pehli nazar me commenting simple lagti hai, lekin naye programmers aksar inhe **galat tarike se** use karte hain.

## 1. Bure comments (Bad comments)
Beginners comments me ye likhte hain ki **"code me kya ho raha hai":**

```javascript
// This code will do this thing (...) and that thing (...)
// ...and who knows what else...
very;
complex;
code;
```

Achhe code me aise **"explanatory" comments bahut kam** hone chahiye. Code bina comments ke bhi samajh aana chahiye.

**Ek badhiya rule:** *"Agar code itna unclear hai ki comment ki zarurat pad rahi hai, to shayad code ko dobara likhna chahiye."*

### Tarika 1: Function alag nikalo (factor out)
Kabhi kabhi code ke tukde ko function se replace karna faydemand hota hai.

**Comment ke saath (theek nahi):**
```javascript
function showPrimes(n) {
  nextPrime:
  for (let i = 2; i < n; i++) {

    // check if i is a prime number
    for (let j = 2; j < i; j++) {
      if (i % j == 0) continue nextPrime;
    }

    alert(i);
  }
}
```

**Alag function `isPrime` ke saath (behtar):**
```javascript
function showPrimes(n) {

  for (let i = 2; i < n; i++) {
    if (!isPrime(i)) continue;

    alert(i);
  }
}

function isPrime(n) {
  for (let i = 2; i < n; i++) {
    if (n % i == 0) return false;
  }

  return true;
}
```

Ab code aasani se samajh aata hai. **Function khud ek comment ban jata hai.** Aise code ko **self-descriptive** kehte hain.

### Tarika 2: Functions banao
Agar lamba "code sheet" ho:

```javascript
// here we add whiskey
for(let i = 0; i < 10; i++) {
  let drop = getWhiskey();
  smell(drop);
  add(drop, glass);
}

// here we add juice
for(let t = 0; t < 3; t++) {
  let tomato = getTomato();
  examine(tomato);
  let juice = press(tomato);
  add(juice, glass);
}

// ...
```

To use functions me refactor karna behtar hai:

```javascript
addWhiskey(glass);
addJuice(glass);

function addWhiskey(container) {
  for(let i = 0; i < 10; i++) {
    let drop = getWhiskey();
    //...
  }
}

function addJuice(container) {
  for(let t = 0; t < 3; t++) {
    let tomato = getTomato();
    //...
  }
}
```

Phir se, functions khud batate hain ki kya ho raha hai. Comment karne ko kuch bacha hi nahi. Code ka **structure bhi behtar** ho jata hai: pata chalta hai har function kya karta hai, kya leta hai aur kya return karta hai.

**Sach ye hai:** "explanatory" comments ko **poori tarah** nahi hata sakte. Complex algorithms hote hain, aur optimization ke liye smart "tweaks" hote hain. Lekin aam taur par code ko **simple aur self-descriptive** rakhne ki koshish karo.

## 2. Achhe comments (Good comments)
Explanatory comments aksar bure hote hain. To achhe comments kaunse hain?

**Architecture describe karo:**
Components ka high-level overview, wo kaise interact karte hain, alag situations me control flow kya hai. Yaani code ka **"bird's eye view"**. High-level architecture diagrams ke liye **UML** naam ki special language hai, jo seekhne layak hai.

**Function ke parameters aur usage document karo:**
Function document karne ke liye special syntax **JSDoc** hai: usage, parameters, returned value.

```javascript
/**
 * Returns x raised to the n-th power.
 *
 * @param {number} x The number to raise.
 * @param {number} n The power, must be a natural number.
 * @return {number} x raised to the n-th power.
 */
function pow(x, n) {
  ...
}
```

Aise comments se function ka maqsad samajh aata hai aur uska code dekhe bina **sahi tarike se use** kar sakte hain. Kai editors (jaise WebStorm) inhe samajhte hain aur **autocomplete** aur automatic code-checking me use karte hain. **JSDoc 3** jaise tools in comments se **HTML documentation** bhi bana sakte hain. Zyada info: https://jsdoc.app

**Task is tarah kyu solve hua?**
Jo likha hai wo important hai, lekin **jo nahi likha wo aur bhi important** ho sakta hai. Task exactly isi tarike se kyu solve kiya gaya? Kai tarike hon to yahi kyu, khaaskar jab wo sabse obvious na ho?

Aise comments ke bina ye situation ho sakti hai:
1. Tum (ya tumhara colleague) kuch samay pehle ka code kholte ho aur dekhte ho ki wo "suboptimal" hai.
2. Tum sochte ho: "Main tab kitna bewakoof tha, ab kitna samajhdar hoon", aur "zyada obvious aur sahi" variant se dobara likh dete ho.
3. ...Dobara likhne ka mann achha tha. Lekin process me dikhta hai ki "zyada obvious" solution asal me kami wala hai. Tumhe dhundhla sa yaad bhi aata hai kyu, kyunki tum ye bahut pehle try kar chuke ho. Tum sahi variant par wapas aate ho, lekin **time waste ho gaya.**

Jo comments **solution ka reason** batate hain wo bahut important hain. Ye development ko **sahi disha** me jaari rakhne me madad karte hain.

**Code me koi subtle feature hai? Wo kahan use hota hai?**
Agar code me kuch **subtle aur counter-intuitive** hai, to use zaroor comment karna chahiye.

## Summary
Achhe developer ki ek important nishani hai **comments: unki maujoodgi aur unki gair-maujoodgi bhi.**

Achhe comments se hum code ko **achhe se maintain** kar sakte hain, der ke baad usme wapas aa sakte hain, aur use zyada effectively use kar sakte hain.

### Ye comment karo:
- **Overall architecture**, high-level view
- **Function ka usage**
- **Important solutions**, khaaskar jab wo turant obvious na ho

### In comments se bacho:
- Jo batate hain **"code kaise kaam karta hai"** aur **"kya karta hai"**
- Unhe tabhi daalo jab code ko itna simple aur self-descriptive banana **namumkin** ho ki comment ki zarurat hi na pade.

Comments auto-documenting tools (jaise **JSDoc3**) ke liye bhi use hote hain: wo unhe padhkar HTML docs (ya kisi aur format me docs) bana dete hain.

## Quick Summary

| Baat | Kya karein |
|------|-----------|
| "Kya ho raha hai" batane wale comments | **Avoid** karo, code ko clear likho |
| Code ka tukda samajhne me mushkil | **Function me alag** nikalo (naam khud comment ban jayega) |
| Architecture / big picture | **Comment karo** |
| Function ka usage, parameters, return | **JSDoc** se document karo |
| "Ye tarika kyu chuna" | **Zaroor comment karo** |
| Subtle ya ulti samajh wali cheez | **Zaroor comment karo** |

**Yaad rakho:** Comment code ke "what" ke liye nahi, uske **"why"** ke liye likho.
