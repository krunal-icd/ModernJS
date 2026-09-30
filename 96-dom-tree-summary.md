# 16. DOM Tree

## Basic idea
HTML ka **har tag ek object** hota hai. Nested tags = "children". Tag ke andar ka text bhi object hota hai. JS se in sab ko access/modify kar sakte ho.
```js
document.body.style.background = 'red';
setTimeout(() => document.body.style.background = '', 3000);
```
Aur properties: `innerHTML`, `offsetWidth`, etc.

## DOM = Tree
```html
<html>
  <head><title>About elk</title></head>
  <body>The truth about elk.</body>
</html>
```
- Tags = **element nodes**, jo tree banate hain (`<html>` root, `<head>` aur `<body>` uske children).
- Tag ke andar ka text = **text node** (`#text`). Ye hamesha leaf hota hai (uske children nahi hote).

**Spaces aur newlines bhi text nodes bante hain!** Jaise `<head>` ke andar `<title>` se pehle ki khaali jagah.

Do exceptions:
1. `<head>` se pehle ke spaces ignore hote hain.
2. `</body>` ke baad kuch likhoge to wo `body` ke andar aakhir me chala jaata hai.

## Autocorrection
Galat HTML ko browser khud theek karke DOM banata hai:
- `<html>`, `<body>` na ho to bana deta hai.
- Unclosed tags (jaise `<li>`) band kar deta hai.
- **Table me `<tbody>` hamesha add hota hai**, chahe HTML me na likha ho.

## Baaki node types
- **Comment node** (`#comment`): comments bhi DOM me aate hain.
- `<!DOCTYPE>` bhi ek node hai.
- `document` object bhi formally ek node hai.

Total 12 node types hain, par 4 hi kaam aate hain:
1. `document` (entry point)
2. element nodes (tags)
3. text nodes
4. comment nodes

## DOM ko dekhna
- Browser DevTools > **Elements** tab (right-click > Inspect).
- Sub-tabs: **Styles** (applied CSS), **Computed** (final CSS per property), **Event Listeners**.
- Live DOM Viewer bhi ek tool hai.

## Console ke saath
- Elements me kuch select karo, phir console me `$0` us selected element ko deta hai (`$1` pichhla).
```js
$0.style.background = 'red';
```
- Ulta: `inspect(node)` node ko Elements tab me dikhata hai.

## Yaad rakho
HTML ka har hissa (tags, text, comments) DOM me ek node hai. DevTools DOM samajhne ke liye best tool hai.
