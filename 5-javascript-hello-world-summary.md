# Hello, World! – Simple Hinglish Summary

Source: https://javascript.info/hello-world

## Main baat
Ye part **core JavaScript** (language khud) ke bare me hai. Script chalane ke liye hum **browser** use karenge kyunki tutorial online hai. Browser-specific cheezein (jaise `alert`) kam se kam use hongi. Agar Node.js (server) me chalana ho to command hai: `node my.js`

## 1. `<script>` tag
JavaScript code ko HTML me `<script>` tag ke andar likhte hain. Browser jab is tag ko padhta hai, code **automatically run** ho jata hai.

```html
<!DOCTYPE HTML>
<html>
<body>
  <p>Before the script...</p>

  <script>
    alert( 'Hello, world!' );
  </script>

  <p>...After the script.</p>
</body>
</html>
```

## 2. Modern markup (purani cheezein jo ab nahi chahiye)
- **`type` attribute:** Pehle `type="text/javascript"` likhna padta tha. **Ab zarurat nahi.** (Ab ye modules ke liye use hota hai.)
- **`language` attribute:** Ab bekaar hai, kyunki JavaScript hi default language hai.
- **Script ke andar `<!-- ... //-->` comments:** Bahut purane browsers ke liye tha. Agar kisi code me dikhe, to samajh lo **code bahut purana hai.**

## 3. External scripts
Zyada code ho to alag file me rakho aur `src` se attach karo:

```html
<script src="/path/to/script.js"></script>
```

- **Path ke types:**
  - Absolute path: `/path/to/script.js` (site root se)
  - Relative path: `script.js` ya `./script.js` (current folder)
  - Full URL: `https://cdnjs.cloudflare.com/.../lodash.js`
- **Multiple scripts:** Multiple `<script>` tags use karo.
- **Fayda:** Browser file ko **cache** kar leta hai. Dusre pages par wahi file dobara download nahi hoti, isse **page fast** hota hai aur traffic kam hota hai.
- Sirf simple scripts HTML me likho, complex wale alag file me.

### Important rule
Agar `src` diya hai, to tag ke **andar likha code ignore ho jata hai.**

```html
<!-- Ye kaam NAHI karega -->
<script src="file.js">
  alert(1); // ignore ho jayega
</script>

<!-- Ye sahi hai: do alag tags -->
<script src="file.js"></script>
<script>
  alert(1);
</script>
```

## Tasks (Practice)
1. **Alert dikhao:** Ek page banao jo "I'm JavaScript!" message dikhaye.
   ```html
   <script>
     alert( "I'm JavaScript!" );
   </script>
   ```
2. **External file se alert:** Upar wale code ko `alert.js` file me daalo aur HTML me `src` se attach karo.
   ```html
   <script src="alert.js"></script>
   ```
   `alert.js` me: `alert("I'm JavaScript!");`

## Quick Summary
- JS code ko page me daalne ke liye **`<script>` tag** use karo.
- **`type` aur `language`** attributes ki ab zarurat nahi.
- Alag file ka code: **`<script src="path/to/script.js"></script>`**
- `src` ke saath tag ke andar code nahi likh sakte, dono ke liye alag tags banao.
