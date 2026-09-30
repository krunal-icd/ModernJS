# 04. Async Iteration aur Async Generators

## Ek line me
Jab data **dheere-dheere / delay se** aata ho (network, timer), to `for await..of` aur async generators use karte hain.

## Async Iterable (manual tarika)
Normal iterable se farq:

| | Normal | Async |
|---|---|---|
| Method | `Symbol.iterator` | `Symbol.asyncIterator` |
| `next()` return | seedha object | **Promise** jo object deta hai |
| Loop | `for..of` | `for await..of` |

```js
let range = {
  from: 1, to: 5,
  [Symbol.asyncIterator]() {
    return {
      current: this.from, last: this.to,
      async next() {
        await new Promise(r => setTimeout(r, 1000));
        return this.current <= this.last
          ? { done: false, value: this.current++ }
          : { done: true };
      }
    };
  }
};

(async () => {
  for await (let v of range) console.log(v); // 1 sec ke gap se 1..5
})();
```
Spread `...` async iterable par **kaam nahi karta** (wo sync iterator maangta hai).

## Async Generator (aasan tarika)
`async function*` likho, andar `await` bhi use kar sakte ho:
```js
async function* generateSequence(start, end) {
  for (let i = start; i <= end; i++) {
    await new Promise(r => setTimeout(r, 1000));
    yield i;
  }
}

(async () => {
  for await (let v of generateSequence(1, 5)) console.log(v);
})();
```
Yaha `generator.next()` promise return karta hai, isliye `await` chahiye.

Object me bhi use kar sakte ho:
```js
async *[Symbol.asyncIterator]() { ... }
```

## Real-life Example: Paginated Data
GitHub commits jaise API pages me data dete hain. Async generator se saari pagination chhup jaati hai:
```js
async function* fetchCommits(repo) {
  let url = `https://api.github.com/repos/${repo}/commits`;
  while (url) {
    const response = await fetch(url);
    const body = await response.json();
    // agla page ka URL 'Link' header me hota hai
    let next = response.headers.get('Link').match(/<(.*?)>; rel="next"/);
    url = next?.[1];
    for (let commit of body) yield commit;
  }
}

for await (let commit of fetchCommits("user/repo")) { /* use commit */ }
```
Bahar se sirf simple loop dikhta hai, andar page-by-page fetch hota rehta hai.

## Yaad rakho
- Delay wala data = async iterator / async generator + `for await..of`.
- Async generator declaration: `async function*`.
- Big files / streams process karne me ye kaafi kaam aata hai.
