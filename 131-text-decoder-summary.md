# 51. TextDecoder aur TextEncoder

Agar binary data asal me **string** ho (jaise text file mili), to use JS string me padhne ke liye `TextDecoder` hai. Ulta (string se bytes) ke liye `TextEncoder`.

## `TextDecoder`
```js
let decoder = new TextDecoder([label], [options]);
```
- **`label`**: encoding. Default `utf-8`, par `big5`, `windows-1251` waghera bhi chalte hain.
- **`options`**:
  - `fatal`: `true` ho to invalid characters par **exception**, warna (default) unhe `\uFFFD` se badal deta hai.
  - `ignoreBOM`: byte-order mark ignore karna (kam zarurat).

Decode karna:
```js
let str = decoder.decode([input], [options]);
```
- `input`: `BufferSource` (`ArrayBuffer` ya uska view)
- `options.stream`: `true` jab data **tukdon (chunks)** me aa raha ho. Kabhi kabhi ek multi-byte character do chunks me kat jaata hai, ye option adhoore character ko yaad rakh ke agle chunk ke saath decode karta hai.

Examples:
```js
new TextDecoder().decode(new Uint8Array([72, 101, 108, 108, 111]));       // Hello
new TextDecoder().decode(new Uint8Array([228, 189, 160, 229, 165, 189])); // 你好
```

Buffer ka sirf ek hissa decode karna ho to `subarray` (bina copy) use karo:
```js
let uint8Array = new Uint8Array([0, 72, 101, 108, 108, 111, 0]);
let binaryString = uint8Array.subarray(1, -1);
new TextDecoder().decode(binaryString);   // Hello
```

## `TextEncoder`
String ko bytes me badalta hai. **Sirf `utf-8`** support karta hai.
```js
let encoder = new TextEncoder();
encoder.encode("Hello");                 // Uint8Array [72,101,108,108,111]
encoder.encodeInto(str, destination);    // destination Uint8Array hona chahiye
```

## Yaad rakho
- Bytes -> string: `TextDecoder`
- String -> bytes: `TextEncoder`
- Chunked data me `stream: true` zaruri.
