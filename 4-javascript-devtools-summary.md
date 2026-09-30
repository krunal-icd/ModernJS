# Developer Console – Simple Hinglish Summary

Source: https://javascript.info/devtools

## Main baat
Code me **errors zaroor aayenge.** Lekin browser me errors normal user ko **by default dikhte nahi hain**, isliye pata nahi chalta ki kya toota hai. Isi ke liye browsers me **Developer Tools** hote hain, jinse errors dekh sakte hain aur JavaScript commands chala sakte hain.

Zyadatar developers **Chrome ya Firefox** use karte hain kyunki inke dev tools sabse best hain.

## 1. Google Chrome
- **Windows/Linux:** `F12` dabao
- **Mac:** `Cmd + Opt + J`
- Dev tools by default **Console tab** me khulte hain.
- Console me kya dikhta hai:
  - **Laal (red) error message**, jaise unknown command
  - Right side me **clickable link** (jaise `bug.html:12`) jo batata hai error kis **line number** par hai
  - Neeche **blue `>` symbol** hota hai. Yahan aap **JavaScript commands type karke `Enter`** dabake chala sakte ho.
- **Multi-line code:** `Shift + Enter` dabao. Sirf `Enter` dabane se code turant run ho jata hai.

## 2. Firefox, Edge aur dusre browsers
- Zyadatar me `F12` se dev tools khulte hain.
- Look & feel almost same hota hai. Ek seekh liya to dusra aasani se aa jata hai (Chrome se start kar sakte ho).

## 3. Safari (sirf Mac)
- Pehle **Develop menu enable** karna padta hai:
  - **Settings → Advanced** me jao aur neeche wala checkbox tick karo.
- Uske baad `Cmd + Opt + C` se console khulta hai, aur top menu me **"Develop"** naam ka naya option aa jata hai.

## Quick Summary
| Browser | Shortcut |
|---------|----------|
| Chrome (Windows) | `F12` |
| Chrome (Mac) | `Cmd + Opt + J` |
| Firefox / Edge | `F12` |
| Safari (Mac) | `Cmd + Opt + C` (pehle Develop menu enable karo) |

**Dev tools se kya kar sakte ho:** errors dekhna, commands chalana, variables check karna, aur bahut kuch.

Ab environment ready hai. Agle section me actual JavaScript seekhna shuru hoga.
