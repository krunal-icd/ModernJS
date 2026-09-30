# 56. Fetch: Download Progress

`fetch` se **download** ki progress track kar sakte hain. **Upload progress ke liye fetch me abhi tarika nahi hai**, uske liye `XMLHttpRequest` use karo.

## Kaise?
`response.body` ek **`ReadableStream`** hai, jo body ko **chunk-by-chunk** deta hai. `response.text()`/`json()` ke ulta ye poora control deta hai aur hum gin sakte hain ki kitna mil chuka.

```js
const reader = response.body.getReader();

while (true) {
  const { done, value } = await reader.read();   // value = Uint8Array chunk
  if (done) break;
  console.log(`Received ${value.length} bytes`);
}
```
`reader.read()` ka result:
- `done`: reading poori ho gayi to `true`
- `value`: bytes ka `Uint8Array`

(Streams API me `for await..of` bhi hai par abhi widely supported nahi, isliye `while` loop.)

## Poora example (progress + result)
```js
// Step 1: fetch shuru aur reader lo
let response = await fetch('https://api.github.com/repos/javascript-tutorial/en.javascript.info/commits?per_page=100');
const reader = response.body.getReader();

// Step 2: total length
const contentLength = +response.headers.get('Content-Length');

// Step 3: data padho
let receivedLength = 0;
let chunks = [];
while (true) {
  const { done, value } = await reader.read();
  if (done) break;

  chunks.push(value);
  receivedLength += value.length;
  console.log(`Received ${receivedLength} of ${contentLength}`);
}

// Step 4: chunks ko ek Uint8Array me jodo
let chunksAll = new Uint8Array(receivedLength);
let position = 0;
for (let chunk of chunks) {
  chunksAll.set(chunk, position);
  position += chunk.length;
}

// Step 5: string me decode
let result = new TextDecoder("utf-8").decode(chunksAll);
let commits = JSON.parse(result);
```

## Steps samjho
1. `response.json()` ke bajay `response.body.getReader()`. **Reader aur response ke methods dono ek hi response par nahi chal sakte**, ek hi use karo.
2. Padhne se pehle `Content-Length` header se total size pata karo. Cross-origin requests me ye na bhi mile, aur technically server ko dena zaruri nahi, par aam taur par hota hai.
3. `reader.read()` tab tak chalao jab tak `done` na ho. Chunks ek array me jama karo (kyunki body consume hone ke baad dobara `json()` se nahi padh sakte).
4. Chunks ko jodne ka koi single method nahi, isliye khud: `new Uint8Array(receivedLength)` + `.set(chunk, position)`.
5. Bytes ko string me badalne ke liye `TextDecoder`, phir chahiye to `JSON.parse`.

Binary chahiye to steps 4 aur 5 ki jagah ek line:
```js
let blob = new Blob(chunks);
```

## Dhyan
- Ye **download** progress hai, upload nahi.
- Size pata na ho to loop me `receivedLength` check karke ek limit par `break` karo, taaki `chunks` memory overflow na kare.
