# 90. Backreferences: `\N` aur `\k<name>`

Capturing groups `(...)` ka content sirf result ya replacement me nahi, **pattern ke andar** bhi use kar sakte hain.

## Number se: `\N`
Pattern me group ko `\N` se reference karte hain (`N` = group number).

**Task:** single-quoted `'...'` ya double-quoted `"..."` dono tarah ki quoted strings dhundhna.

Dono quotes ko brackets me daal ke `['"](.*?)['"]` likhein to mixed quotes (`"...'` ya `'..."`) bhi mil jaate hain. Jaise string `"She's the one!"` me galat match:
```js
let str = `He said: "She's the one!".`;
str.match(/['"](.*?)['"]/g);   // "She'
```
Pattern ne opening `"` liya aur text `'` tak khaya jo closing ban gaya.

Fix: closing quote **bilkul wahi** hona chahiye jo opening tha. Opening quote ko capturing group me daalo aur `\1` se reference karo: `(['"])(.*?)\1`:
```js
str.match(/(['"])(.*?)\1/g);   // "She's the one!"
```
Engine pehla quote `(['"])` dhundh ke uska content yaad rakhta hai (pehla group). Pattern me aage `\1` = "wahi text dhundho jo pehle group me tha". `\2` doosre group ka content, `\3` teesre ka.

Dhyan:
- Group me `?:` ho (non-capturing) to use reference nahi kar sakte, engine use yaad nahi rakhta.
- **Pattern me `\1`, replacement me `$1`.** (Replacement me dollar, pattern me backslash.)

## Naam se: `\k<name>`
Kai parentheses ho to naam dena convenient hai. Named group ko `\k<name>` se reference karo:
```js
let regexp = /(?<quote>['"])(.*?)\k<quote>/g;
`He said: "She's the one!".`.match(regexp);   // "She's the one!"
```
