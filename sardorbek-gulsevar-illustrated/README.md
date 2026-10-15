# Sardorbek & Gulsevar — Premium taklifnoma

**25-oktabr 2026, yakshanba · 12:00–14:00 (UTC+5)**  
**Zulfiya to‘yxonasi, Buvayda tumani, Farg‘ona viloyati**

Yuborgan uchta suratingiz, to‘yxona tashqi ko‘rinishining badiiy chizmasi va Izzat Shukurovning “Vafodorim” MP3 fayli kiritilgan. Oldingi xohishingiz saqlandi: mehmon ismi so‘ralmaydi, RSVP/“Kelaman” ekrani va mehmon hisoblagichi yo‘q.

## Tayyor imkoniyatlar

- Ivory–oltin palitra va bordo urg‘ular; ravoqli asosiy surat.
- 3D CSS konvert, qopqoq ochilishi va ichidan chiqadigan xat; bitta ochish tugmasi.
- Desktopda suratlar yengil 3D qiyshayadi; telefonda uchta suratli galereya suriladi.
- Three.js bilan oltin zarralar va aylanuvchi uzuklar; GSAP bilan harflarning ketma-ket chiqishi va scroll animatsiyalari. Internet/CDN/WebGL ishlamasa asosiy sayt davom etadi.
- Musiqa saytning o‘zida faqat bosilganda boshlanadi, ovoz silliq kirib-chiqadi; ijroda yurak va notalar chiqadi. Saqlangan xohish keyingi tashrifda avtoplay qilmaydi.
- Kun/soat/daqiqa/soniya hisoblagichi, +05:00 vaqti, tugagan to‘y uchun alohida holat, .ics taqvim fayli.
- Google Maps, Yandex Maps, manzilni nusxalash, ulashish, to‘liq ekran tugmasi.
- To‘y dasturi: tasdiqlangan 12:00 boshlanish va 14:00 yakun. Boshqa marosim vaqtlari taxmin qilib qo‘shilmagan.
- Nafis tabrik formasi; Telegramga ikki xil ulanish tartibi quyida.

## GitHub Pages’ga joylash

1. Papkaning ichidagi `index.html`, `style.css`, `app.js`, `motion.js`, `config.js`, `.nojekyll` va **assets** papkasini repozitoriy ildiziga yuklang.
2. Settings → Pages → Deploy from a branch → main → / (root).
3. Havola tayyor bo‘lgach, `config.js` ichidagi `siteUrl`ga to‘liq havolani kiriting.
4. Ulashish kartochkasi uchun **index.html** ichidagi `og:url` va `og:image`ni ham mutlaq URL bilan yozing. Masalan, `https://USERNAME.github.io/REPO/` va `https://USERNAME.github.io/REPO/assets/og.jpg`. Telegram kabi crawlerlar JS orqali yangilangan metadata’ni har doim o‘qimaydi.

Build, npm install yoki alohida frontend server kerak emas. `server/` papkasini GitHub Pages’ga yuklash shart emas. Sayt bu topshiriqda internetga joylanmagan.

Faylni ikki marta bosib ochish asosiy sahifani ko‘rsatadi, lekin brauzer `file://` ostida module/WebGL kutubxonalari va ayrim API’larni cheklashi mumkin. To‘liq animatsiyalarni HTTPS manzilida tekshiring.

## Telegram — hozirgi holat va avtomatik yuborish

**Hozir `apiBase` bo‘sh:** tabrik matni bilan `@anthophile_dm` chat havolasi ochiladi, mehmon Telegram ichida yuboradi. Sayt bu holatda “yuborildi” demaydi. Bu variant bot tokenisiz ishlaydi.

**Qo‘shimcha Telegram tugmasisiz yuborish uchun:** `server/worker.mjs` kodi tayyor. Worker va sirlar quyidagicha sozlangach, `config.js`dagi `apiBase`ni Worker HTTPS URL’iga o‘zgartiring. Forma o‘z API’ingiz orqali botga yuboradi va faqat Telegram muvaffaqiyatni tasdiqlagach “Yuborildi” deydi.

### Cloudflare Worker’ni ulash

1. Telegram’dagi rasmiy **@BotFather** orqali bot yarating. Tokenni faqat Cloudflare secret sifatida saqlang. Uni config.js, HTML, GitHub yoki chatga qo‘ymang.
2. Tabriklarni qabul qiladigan shaxs yangi botni ochib **Start** bosishi kerak. Shaxsiy profil uchun `@anthophile_dm`ning o‘zi botga yetarli emas; bot bilan suhbatning raqamli chat ID’sini oling. Buni Bot API `getUpdates` javobidagi `message.chat.id` orqali o‘zingizning xavfsiz muhitingizda olish mumkin. Tokenli URL’larni ulashmang.
3. Cloudflare hisobingizda Workers/Durable Objects yoqilgan bo‘lsin. Wrangler’ni rasmiy yo‘l bilan o‘rnating va tizimga kiring.
4. Terminalda `server/` papkasidan quyidagilarni bajaring (sirlarni interaktiv so‘ralganda kiriting):

```sh
wrangler secret put TELEGRAM_BOT_TOKEN
wrangler secret put TELEGRAM_CHAT_ID
wrangler secret put SIGNING_SECRET
```

`SIGNING_SECRET` uchun kamida 32 baytlik tasodifiy maxfiy qiymat ishlating.

5. `wrangler.toml`dagi `ALLOWED_ORIGIN`ni saytingizning aynan origin qiymatiga almashtiring: `https://USERNAME.github.io`. Repozitoriy yo‘li yoki yakuniy slash qo‘shmang. Maxsus domen bo‘lsa o‘sha domenni yozing.
6. `wrangler deploy` orqali Worker’ni joylang. Durable Object binding va migration faylda bor.
7. Worker URL’ini `config.js` → `apiBase`ga kiriting: `https://sardorbek-gulsevar-wishes.ACCOUNT.workers.dev`.
8. Saytni yangilab, haqiqiy tabrik bilan oxirigacha sinang. Bot tokeni, chat ID yoki Worker deployment bu topshiriqda mavjud bo‘lmagani sababli jonli Telegram yuborish tekshirilmagan.

API: `GET /api/challenge`, `POST /api/wish`. `/api/rsvp` serverda mavjud, ammo **o‘chirilgan** (`ENABLE_RSVP=false`) va frontendda shakli yo‘q. Uni yoqish alohida ish; `config.js`dagi enableRsvp faqat kelajak uchun belgi, ekran yaratmaydi.

### Server himoyasi

Aniq origin CORS, honeypot, server imzolagan IP’ga bog‘langan 3 soniyalik challenge, 30 daqiqalik amal muddati, Durable Object ichida atomik rate-limit (bir IP uchun 10 daqiqada 3 yuborish), ishlatilgan nonce’ni qayta qabul qilmaslik, 16 KB body va 3–1800 belgi chegarasi, HTML escape, maxfiy token faqat serverda. CORS autentifikatsiya o‘rnini bosmaydi; katta ochiq kampaniya uchun qo‘shimcha bot himoyasi kerak bo‘lishi mumkin.

## O‘zgartirishlar — config.js

- `groom`, `bride`: ismlar.
- `startsAt`, `endsAt`: `2026-10-25T12:00:00+05:00` kabi ISO sana/vaqt; taqvim va hisoblagich shundan olinadi.
- `venue`, `address`, `landmark`, `coordinates`: to‘yxona va xarita.
- `phone`: tasdiqlangan raqam; bo‘sh bo‘lsa havola chiqmaydi.
- `parents`, `dressCode`: ixtiyoriy ma’lumotlar; berilmagani uchun bo‘sh.
- `program`: vaqt, sarlavha va izohlar. Sahifada shu tartibda chiqadi.
- `photos.hero`, `photos.detail`, `photos.story`: uchta surat yo‘li.
- `music`: MP3 yo‘li.
- `telegram`, `apiBase`, `siteUrl`: aloqa va joylangan manzil. **Sir saqlamang.**

Asosiy statik HTML yozuvlarini ham sinxron yangilang: JavaScript o‘chirilganda va link preview’da ular ko‘rinadi. Monogramma SVG va ikonkalar S&G uchun chizilgan; juftlik o‘zgarsa ularni ham yangilang.

## Fayllar

- `assets/music.mp3` — taqdim etilgan “Vafodorim”, dekodlash tekshiruvidan o‘tdi.
- `assets/photo-hero.jpg`, `photo-detail.jpg`, `photo-story.jpg` — taqdim etilgan uch surat, mazmuni o‘zgartirilmagan.
- `assets/zulfiya-illustration.png` — sayt uchun yaratilgan akvarel uslubidagi badiiy tashqi ko‘rinish. Haqiqiy fasadning hujjatli surati emas.
- `assets/botanical.png` — oldingi bosqichda AI yordamida yaratilgan bezak.
- `assets/og.jpg` — 1200×630 ulashish kartochkasi.
- `assets/favicon.svg`, `assets/apple-touch-icon.png` — S&G belgisi.

Musiqani ommaviy saytda ishlatish uchun tegishli ruxsatni tekshiring. Muqobil sifatida saytda foydalanishga ruxsat beruvchi royalty-free instrumental trek qo‘yish mumkin; royalty-free har doim mualliflik huquqisiz degani emas.

## Kutubxonalar va tekshiruv chegaralari

Three.js **0.169.0**, GSAP/ScrollTrigger **3.12.5**, Lenis **1.1.14** — aniq eski versiyalarga biriktirilgan jsDelivr URL’lari. Ular ishlamasa oddiy CSS/JS ko‘rinishi saqlanadi. Google Fonts ishlamasa tizim shrifti bor.

JavaScript sintaksisi, kod modelidagi frontend ishga tushishi, audio holatlari, vaqtlar, fayllar, HTML asoslari va server himoyalari tekshirildi. Telegram chaqiruvlari testda taqlid qilindi; hech qanday haqiqiy xabar yuborilmadi. Brauzer xavfsizlik siyosati mahalliy fayl ko‘rinishini blokladi. **Haqiqiy brauzer ko‘rinishi, mobil FPS va Lighthouse 90+ natijasi tasdiqlanmagan.**


## Yangi harakatlar — Motion nashri

`enhancements.css` va `enhancements.js` ham sayt bilan yuklanishi kerak. Tanlangan sarlavha va ismlarda oltin nur yuradi; suratlar mayin suzadi, ramkalar yaltiraydi, surat ustidan yorug‘lik o‘tadi. Sana, muhr, yurak bezaklari, tugmalar va yakun monogrammasi sokin jonlanadi. Uch bo‘limda mayin gulbarglar bor. Bu yangi effektlar tashqi kutubxonasiz ishlaydi, faylni oddiy brauzerda ochganda ham ko‘rinadi.

Ekrandan chiqib ketgan bo‘limlar va yashirilgan tabdagi CSS harakatlar pauza qilinadi. Telefonlarda gulbarglar kamaytiriladi. “Harakatni kamaytirish” sozlamasi yoqilgan bo‘lsa effektlar o‘chadi. Mavjud 3D tilt va scroll o‘tishlari bilan transform to‘qnashmasligi uchun yangi suzishlar alohida translate/scale xususiyatlaridan foydalanadi.

Kod va fayl bog‘lanishlari tekshirildi. Oldingi brauzer cheklovi sabab yangi effektlarning vizual ko‘rinishi bu muhitda tekshirilmagan.


## To‘yxona tasviri yangilandi

Ichki zal surati o‘rniga iliq krem, shampan oltin, bordo va mayin yashil ranglardagi chizilgan tashqi ko‘rinish qo‘yildi. Bu “Zulfiya” uchun yaratilgan tasavvuriy fasad; haqiqiy bino ko‘rinishi sifatida taqdim etilmaydi. Manzil, xarita, vaqt, juftlik suratlari, musiqa va tabrik funksiyalari o‘zgarmadi.
