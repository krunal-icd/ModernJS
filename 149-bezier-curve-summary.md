# 69. Bezier Curve

Bezier curves computer graphics me shapes banane, CSS animation aur bahut jagah kaam aati hain. Chhoti si cheez hai, ek baar samajh lo to vector graphics aur animations me comfortable ho jaate ho.

Ye theory wala chapter hai, agla chapter (CSS animations) me dikhega ki CSS me kaise use hoti hain.

## Control points
Bezier curve **control points** se define hoti hai. 2, 3, 4 ya zyada ho sakte hain.
- 2 points: linear curve (seedhi line)
- 3 points: quadratic (parabola jaisi)
- 4 points: cubic

Dhyan dene wali baatein:
1. **Points hamesha curve par nahi hote** (normal hai).
2. **Curve ka order = points - 1.**
3. **Curve hamesha control points ke convex hull ke andar rehti hai.** Isse computer graphics me intersection test tez hota hai: convex hulls intersect na karein to curves bhi nahi.

**Drawing ke liye asli fayda:** points hilane par curve **intuitively** badalti hai. Curve tangent lines `1 -> 2` aur `3 -> 4` ki taraf khinchti hai. Kai curves jod ke lagbhag kuch bhi (car, letters, vase) bana sakte ho.

## De Casteljau's algorithm
Mathematical formula ke barabar hai aur dikhata hai ki curve **kaise bani**.

### 3 points ke liye
1. Control points draw karo: `1`, `2`, `3`.
2. Points ko segments se jodo: `1 -> 2 -> 3` (2 segments).
3. Parameter **`t`** `0` se `1` tak chalta hai (jaise 0, 0.05, 0.1 ... 1). Har `t` ke liye:
   - Har segment par shuru se `t` ke **anupaat** me ek point lo (t=0 par shuru me, t=0.5 par beech me, t=1 par end me). 2 segments ke liye 2 points.
   - Un dono points ko jodo.
4. Is naye segment par bhi `t` ke anupaat me ek point lo.
5. `t` ke har value ke liye aisa ek point bantta hai. Ye sab points milke **Bezier curve** banate hain.

### 4 points ke liye
- Points ko jodo: `1->2`, `2->3`, `3->4` (3 segments).
- Har `t` ke liye in par `t` ke anupaat wale points lo, jodo, to 2 segments.
- Un par phir points lo, 1 segment.
- Us par ek point lo. Ye curve ka point hai.

### General (N points)
1. N points jodo, N-1 segments.
2. Har `t` par har segment par anupaat ka point lo aur jodo: N-2 segments.
3. Tab tak dohrao jab tak ek point na bache.

Ye recursive hai isliye kisi bhi order ki curve ban sakti hai. Par practically 2-3 points hi zyaada kaam ke hain, complex lines ke liye kai curves jod lete hain (aasan hai).

**Points ke *through* curve banana?** Ye alag kaam hai (**interpolation**), Bezier me control points curve par nahi hote (pehla aur aakhri chhodkar). Uske liye Lagrange polynomial, spline interpolation jaise tarike hote hain.

## Maths (formula)
`t` segment `[0, 1]` me, control points `P1, P2, ...`:
- **2 points:** `P = (1-t)P1 + tP2`
- **3 points:** `P = (1-t)^2 P1 + 2(1-t)t P2 + t^2 P3`
- **4 points:** `P = (1-t)^3 P1 + 3(1-t)^2 t P2 + 3(1-t) t^2 P3 + t^3 P4`

Ye **vector equations** hain, `P` ki jagah `x` aur `y` alag alag rakh ke coordinates nikaal sakte hain.

Example: 3 points `(0,0)`, `(0.5, 1)`, `(1, 0)`:
- `x = (1-t)^2*0 + 2(1-t)t*0.5 + t^2*1 = t`
- `y = (1-t)^2*0 + 2(1-t)t*1 + t^2*0 = -2t^2 + 2t`

`t` 0 se 1 tak chalte hue `(x, y)` curve banate hain.

## Summary
- Bezier curves **control points** se define hoti hain.
- Do definitions: drawing process (De Casteljau) aur mathematical formula.
- Achhi baatein: mouse se points hilake smooth lines banti hain, aur kai curves jod ke complex shapes.

**Kahan use hoti hain:**
- Computer graphics, modeling, vector graphic editors (fonts bhi Bezier curves se describe hote hain)
- Web dev me: **Canvas** aur **SVG** graphics
- **CSS animation** me path aur speed describe karne ke liye (agla chapter)
