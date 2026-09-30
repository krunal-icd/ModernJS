# 72. Web Components: Orbital Height se (Introduction)

Ye section "web components" ke modern standards ke bare me hai. Abhi ye standards develop ho rahe hain: kuch achhe se supported aur HTML/DOM standard me integrated hain, kuch abhi draft me. Examples kisi bhi browser me try karo (Google Chrome sabse up-to-date hota hai).

## International Space Station (ISS) se seekh
Component ka idea naya nahi, kai frameworks me use hota hai. ISS ko dekho:
- Bahut saare **components** se bana
- Har component ke andar aur chhote hisse
- Components bahut complex, aam websites se bhi zyada
- Alag alag deshon ki teams, alag alag bhashaon me banati hain
- Phir bhi ye udta hai aur logon ko space me zinda rakhta hai

Itne complex devices kaise bante hain? Unse kaun se principles hum le sakte hain?

## Component architecture
Complex software banane ka jaana-maana rule: **complex software mat banao.** Kuch complex ho jaye to use **saral hisson me todo** aur sabse obvious tarike se jodo.

**Achha architect wo hai jo complex ko simple bana de.**

User interface ko visual components me baant sakte hain: har ek ki page par apni jagah, ek saaf kaam, aur baaki se alag. Twitter jaisi site ko dekho, wo naturally components me tootti hai: top navigation, user info, follow suggestions, submit form, messages. Components ke sub-components bhi ho sakte hain (jaise messages "message list" ka hissa, ya clickable user picture bhi ek component).

Component kya hai ye intuition, experience aur common sense se tay hota hai. Aam taur par ek alag visual entity jise hum describe kar sakein ki wo kya karti hai aur page se kaise interact karti hai.

Ek **component me hota hai:**
- Apni **JavaScript class**
- **DOM structure**, jo sirf uski class manage kare, bahar ka code access na kare (**"encapsulation"** principle)
- Us par lagne wale **CSS styles**
- **API**: events, class methods waghera, dusre components se interact karne ke liye

Component koi khaas cheez nahi hai. Kai frameworks aur methodologies hain, jinme aksar special CSS classes aur conventions se "component feel" lati hain (CSS scoping aur DOM encapsulation).

**"Web components"** ye sab **browser ki built-in capabilities** se dete hain, isliye emulate nahi karna padta:
- **Custom elements**: apne khud ke HTML elements banana
- **Shadow DOM**: component ke liye andar ka DOM tree jo baaki se chhupa ho
- **CSS Scoping**: aise styles jo sirf component ke Shadow DOM ke andar lagen
- **Event retargeting** aur kuch chhoti cheezein jo custom components ko behtar fit karti hain

Agla chapter **Custom Elements** par hai, jo web components ka buniyadi aur achhe se supported hissa hai.
