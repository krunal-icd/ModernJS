# 53. File aur FileReader

`File` object **`Blob` se inherit** karta hai, aur filesystem se jude features jodta hai.

## File milne ke 2 tarike
1. Constructor (Blob jaisa):
```js
new File(fileParts, fileName, [options])
```
- `fileParts`: Blob/BufferSource/String ka array
- `fileName`: file ka naam
- `options.lastModified`: last modification ka timestamp

2. Zyaada common: `<input type="file">`, drag'n'drop, ya dusre browser interfaces se (tab info OS se aati hai).

Blob ke properties ke alawa: `name`, `lastModified`.
```html
<input type="file" onchange="showFile(this)">
<script>
function showFile(input) {
  let file = input.files[0];
  alert(`File name: ${file.name}`);
  alert(`Last modified: ${file.lastModified}`);
}
</script>
```
`input.files` array-like hai (kai files chuni ja sakti hain).

## `FileReader`
`Blob` (to `File` bhi) se data **padhne** ke liye. Disk se padhne me time lag sakta hai, isliye **events** se data deta hai.
```js
let reader = new FileReader();   // koi argument nahi
```

### Methods
- `readAsArrayBuffer(blob)`: binary format (`ArrayBuffer`)
- `readAsText(blob, [encoding])`: text string (default `utf-8`)
- `readAsDataURL(blob)`: base64 data URL
- `abort()`: rok do

Kaun sa? Jaisa format chahiye:
- `readAsArrayBuffer`: binary files, low-level operations (slicing jaise high-level kaam ke liye `File` khud Blob hai, padhne ki zarurat nahi)
- `readAsText`: text files
- `readAsDataURL`: `img` ke `src` jaisi jagah. (Iska alternative `URL.createObjectURL(file)` hai.)

### Events
`loadstart`, `progress` (padhte waqt), `load` (bina error ke poora), `abort`, `error`, `loadend` (success ya fail dono me).

Result: `reader.result` (success), `reader.error` (fail). Sabse zyaada `load` aur `error` use hote hain.

```html
<input type="file" onchange="readFile(this)">
<script>
function readFile(input) {
  let file = input.files[0];
  let reader = new FileReader();

  reader.readAsText(file);
  reader.onload = function() { console.log(reader.result); };
  reader.onerror = function() { console.log(reader.error); };
}
</script>
```

`FileReader` sirf files nahi, **kisi bhi blob** ko padh sakta hai, to blob ko dusre format me badalne me bhi kaam aata hai (`readAsText` = `TextDecoder` ka alternative).

**`FileReaderSync`:** Web Workers ke andar synchronous version (events nahi, seedha result). Wahan delay page ko affect nahi karta.

## Summary
- `File` = `Blob` + `name` + `lastModified` + filesystem se padhne ki ability. Aam taur par `<input>` ya drag'n'drop se milta hai.
- `FileReader` 3 formats me padhta hai: string, `ArrayBuffer`, data URL.
- Aksar padhne ki zarurat hi nahi: `URL.createObjectURL(file)` se chhota URL banake `<a>`/`<img>` me lagao.
- Network par bhejna aasan: `XMLHttpRequest`/`fetch` `File` objects seedha lete hain.
