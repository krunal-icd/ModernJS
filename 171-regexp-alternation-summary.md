# 91. Alternation (OR): `|`

Alternation regexp me simple "**OR**" hai, vertical line `|` se.

Programming languages dhundhne hain (HTML, PHP, Java ya JavaScript): `html|php|java(script)?`
```js
let regexp = /html|php|css|java(script)?/gi;
"First HTML appeared, then CSS, then JavaScript".match(regexp);   // 'HTML', 'CSS', 'JavaScript'
```

Square brackets bhi choose karne dete hain (`gr[ae]y` = `gray` ya `grey`), par wo sirf **characters ya character classes** allow karte hain. Alternation **koi bhi expressions** allow karta hai. `A|B|C` = `A`, `B` ya `C` me se koi ek expression.

- `gr(a|e)y` = `gr[ae]y`
- `gra|ey` = `gra` ya `ey`

Pattern ke chune hue hisse par alternation lagane ke liye **parentheses**:
- `I love HTML|CSS` = `I love HTML` ya `CSS`
- `I love (HTML|CSS)` = `I love HTML` ya `I love CSS`

## Example: time ke liye regexp
Simple `\d\d:\d\d` bahut dheela hai (`25:99` bhi accept karta hai). Behtar:
- **Hours:** pehla digit `0` ya `1` ho to agla koi bhi digit (`[01]\d`); pehla digit `2` ho to agla `[0-3]`. Alternation: `[01]\d|2[0-3]`.
- **Minutes:** `00` se `59`: `[0-5]\d`.

Seedha jodne par `[01]\d|2[0-3]:[0-5]\d` galat hai: alternation `[01]\d` **ya** `2[0-3]:[0-5]\d` ke beech ho jaata hai (minutes sirf doosre variant me judte hain). Hours ko parentheses me lo:
```js
let regexp = /([01]\d|2[0-3]):[0-5]\d/g;
"00:00 10:10 23:59 25:99 1:2".match(regexp);   // 00:00,10:10,23:59
```

## Tasks
**Programming languages (`Java JavaScript PHP C++ C`):** seedha `Java|JavaScript|PHP|C|C\+\+` **galat** hai (result `Java,Java,PHP,C,C`), kyunki engine alternatives ek ke baad ek check karta hai: pehle `Java` mil jaata hai to `JavaScript` kabhi nahi milta, `C` pehle to `C++` nahi.
Do fix:
1. **Lambe variants pehle:** `JavaScript|Java|C\+\+|C|PHP`
2. **Same shuruaat wale variants merge:** `Java(Script)?|C(\+\+)?|PHP`

**BB-tag pairs (`[b]...[/b]`, `[url]...[/url]`, `[quote]...[/quote]`), nested ho sakte hain, newlines bhi:**
`/\[(b|url|quote)].*?\[\/\1]/gs`. Opening tag `\[(b|url|quote)]`, lazy `.*?` (`s` flag se newline bhi), aur closing tag backreference `\1` se. Closing tag ka slash bhi escape karna padta hai.

**Quoted strings `"..."` (escaped quotes `\"`, `\\`, `\n` ke saath):** `/"(\\.|[^"\\])*"/g`
- Opening quote `"`
- Backslash ho to uske baad koi bhi character (`\\.`)
- Warna quote aur backslash ke alawa koi bhi character `[^"\\]`
- ... closing quote tak

**Poora `<style...>` tag (par `<styler>` nahi):** `<style(>|\s.*?>)`. Yaani `<style` ke baad ya to seedha `>` ya space aur phir kuch bhi `>` tak.
