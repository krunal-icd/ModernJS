# 36. Scrolling (`scroll` event)

`scroll` event page (`window`) ya kisi scrollable **element** ke scroll par chalta hai.

## Use cases
- Document me position ke hisaab se controls/info dikhana ya chhupana.
- Neeche tak scroll hone par aur data load karna (infinite scroll).

```js
window.addEventListener('scroll', function() {
  document.getElementById('showScroll').innerHTML = window.pageYOffset + 'px';
});
```

## Scroll rokna
`onscroll` me `event.preventDefault()` **kaam nahi karta**, kyunki wo scroll ho jaane ke **baad** chalta hai.

Scroll rokne ke liye us event par `preventDefault()` lagao jo scroll shuru karta hai (jaise `keydown` par `PageUp/PageDown`). Par scroll kai tarike se hota hai, isliye zyada bharosemand tarika **CSS `overflow`** property hai.

## Tasks (short)
- **Endless page:** jab user page ke end se **100px** ke andar aa jaye to naya content jodo. Scroll "elastic" aur imprecise hota hai, isliye exact end ka wait mat karo.
  ```js
  function populate() {
    while (true) {
      let windowRelativeBottom = document.documentElement.getBoundingClientRect().bottom;
      if (windowRelativeBottom > document.documentElement.clientHeight + 100) break;
      document.body.insertAdjacentHTML("beforeend", `<p>Date: ${new Date()}</p>`);
    }
  }
  ```
  (Document ka window-relative `bottom` kabhi window ki height se kam nahi hota.)
- **Up/down button:** page window ki height se zyada scroll ho to arrow dikhao, click par upar scroll karo.
- **Visible images load karna (lazy load):** `<img src="placeholder.svg" data-src="real.jpg">`. Scroll par `getBoundingClientRect()` se check karo ki image screen me hai, to `src = dataset.src`. Page load par bhi ek baar chalao.
