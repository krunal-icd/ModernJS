# 12. BigInt

## Kya hai?
Ek special number type jo **kitne bhi bade integers** store kar sakta hai (normal Number ki limit se bahar).

## Banane ke tarike
```js
const a = 1234567890123456789012345678901234567890n;   // end me n
const b = BigInt("1234567890123456789012345678901234567890");
const c = BigInt(10);   // 10n
```

## Math
```js
1n + 2n;   // 3n
5n / 2n;   // 2n  (decimal hissa hat jaata hai, zero ki taraf round)
```
Sab operations bigint hi return karte hain.

## Mix nahi kar sakte
```js
1n + 2;   // Error: Cannot mix BigInt and other types
```
Convert karna padega:
```js
BigInt(2) + 1n;      // number -> bigint
Number(1n) + 2;      // bigint -> number
```
Dhyan: bahut bada bigint `Number` me convert karoge to extra bits **cut** ho jaate hain (error nahi aata).

Unary plus `+bigint` **allowed nahi** hai. `Number()` use karo.

## Comparison
```js
2n > 1;      // true (< > chalte hain)
1 == 1n;     // true
1 === 1n;    // false (alag types)
```

## Boolean me
- `0n` falsy hai, baaki sab truthy.
```js
0n || 2;   // 2
1n || 2;   // 1n
```

## Polyfill
BigInt ka achha polyfill mushkil hai (operators ka behavior alag hota hai). Isliye **JSBI** library hai: `JSBI.add(a, b)` jaise calls likhte ho, aur Babel plugin ise native bigint me badal deta hai.

## Yaad rakho
- Number ke saath mix nahi.
- `n` suffix ya `BigInt()` se banao.
- Bahut bade integers (jaise IDs, cryptography) ke liye useful.
