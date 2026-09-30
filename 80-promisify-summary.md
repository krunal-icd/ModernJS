# Promisification

## Ye kya hai?
Callback lene wale function ko aise function me badalna jo **promise return kare**.

Kyun? Bahut se purane functions/libraries callback-based hain, par promises (aur `async/await`) zyada aasan hain.

## Ek function ko promisify karna
Callback-based `loadScript(src, callback)`:
```javascript
function loadScript(src, callback) {
  let script = document.createElement("script");
  script.src = src;
  script.onload = () => callback(null, script);
  script.onerror = () => callback(new Error(`Load error: ${src}`));
  document.head.append(script);
}
```
Iska promise version, ye ek **wrapper** hai jo apna callback deta hai aur uske andar `resolve/reject` chalata hai:
```javascript
let loadScriptPromise = function (src) {
  return new Promise((resolve, reject) => {
    loadScript(src, (err, script) => {
      if (err) reject(err);
      else resolve(script);
    });
  });
};

loadScriptPromise("path/script.js").then(...);
```

## Helper: `promisify(f)`
Kai functions ke liye baar-baar likhne ki jagah ek helper:
```javascript
function promisify(f) {
  return function (...args) {
    return new Promise((resolve, reject) => {
      function callback(err, result) {
        if (err) reject(err);
        else resolve(result);
      }
      args.push(callback);   // apna callback aakhir me jodo
      f.call(this, ...args); // original function chalao
    });
  };
}

let loadScriptPromise = promisify(loadScript);
```
Ye maan ke chalta hai ki callback exactly `(err, result)` leta hai (sabse common format).

## Kai results wale callbacks ke liye
Agar callback `callback(err, res1, res2, ...)` jaisa ho:
```javascript
function promisify(f, manyArgs = false) {
  return function (...args) {
    return new Promise((resolve, reject) => {
      function callback(err, ...results) {
        if (err) reject(err);
        else resolve(manyArgs ? results : results[0]);
      }
      args.push(callback);
      f.call(this, ...args);
    });
  };
}

f = promisify(f, true);
f(...).then(arrayOfResults => ...);
```

Jinme `err` hi nahi hota (`callback(result)`) unhe khud manually promisify karna padta hai.

Ready-made options: `es6-promisify` library, aur Node.js me built-in `util.promisify`.

## Dhyan rakho
Promise ka **sirf ek result** hota hai, jabki callback kai baar bhi chal sakta hai. Isliye promisification sirf un functions ke liye hai jo callback **ek hi baar** chalate hain. Baad ki calls ignore ho jaati hain.

Ye callbacks ka poora replacement nahi, par `async/await` ke saath bahut kaam aata hai.

## Yaad rakho
- Promisify = callback wale function ko promise wale me badalna.
- Wrapper apna callback deta hai jo `resolve/reject` chalata hai.
- Sirf ek-baar-chalne-wale callbacks ke liye.
