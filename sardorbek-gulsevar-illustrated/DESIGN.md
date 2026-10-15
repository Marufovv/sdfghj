# Dizayn va talablar holati

Asos: foydalanuvchi tanlagan birinchi ivory–oltin uslub. Yangi bosqichda bordo urg‘u, uchta shaxsiy surat, haqiqiy MP3, ravoqli muqova, CSS 3D konvert, suratlarda tilt, gorizontal galereya, oltin WebGL zarralar va uzuklar qo‘shildi. Yuzlar, suratlar va to‘yxona tuzilishi o‘zgartirilmadi.

## Biriktirilgan texnik topshiriq bilan farqlar

- Foydalanuvchining avvalgi bevosita talabi saqlandi: RSVP va mehmon ismi yo‘q. Serverning RSVP endpoint’i tayyor, lekin o‘chirilgan.
- Bosqichlar 100svh minimum balandlikda; uzun kontent kesilmaydi. Mobil qurilmada majburiy scroll-snap/pin yo‘q: odatiy aylantirish saqlangan.
- Konvert CSS 3D; WebGL fonida uzuklar va particles bor. To‘liq 3D konvert modeli va kamera bilan xat ichiga kirish effekti yo‘q.
- GSAP scrub, yengil desktop Lenis, surat tilt, harflar stagger mavjud. Telefon giroskopi va unga ruxsat so‘rovi qo‘shilmagan.
- Ismlar, sana, manzil, telefon, dastur, ota-onalar, kiyinish uslubi config orqali o‘zgaradi. Telefon, ota-onalar va kiyinish ma’lumotlari berilmagani uchun uydirib qo‘shilmadi.
- Server manzili va sirlar berilmagani sabab avtomatik Telegram ishlashi hali yoqilmagan; hozir aniq izohli draft havolasi mavjud.

## Majburiy tuzatishlar auditi

| Talab | Holat |
|---|---|
| Token/chat ID frontendda bo‘lmasligi | Bajarildi; server env uchun joy bor |
| audio source va MIME | Bajarildi; haqiqiy music.mp3 qo‘shildi |
| To‘liq HTML/CSS/JS/assets | Bajarildi |
| JS o‘chiq bo‘lsa kontent | Asosiy statik matn va surat ko‘rinadi; dinamik dastur konfiguratsiyasi JS talab qiladi |
| Logo #muqova, to‘g‘ri yo‘nalish belgilari | Bajarildi |
| Bitta aria-live/status hududi | Bajarildi |
| Footer main’dan tashqarida | Bajarildi |
| Spamdan himoya | Worker kodida; deployment kerak |
| Google/Yandex, manzil va mo‘ljal | Bajarildi; telefon berilmagan, bo‘sh |
| robots noindex,nofollow | Bajarildi |
| OG/Twitter/favicon/apple icon/preconnect | Fayllar va taglar bor; og:url va mutlaq og:image nashr manziliga mos to‘ldiriladi |
| Musiqa foydalanuvchi bosganda | Bajarildi; fayl dekodlandi, brauzer ijrosi tekshirilmagan |
| 3D fallback va reduced-motion | Kodda mavjud; brauzer QA qilinmagan |
| Past FPS’da sifatni kamaytirish | WebGL renderer’da mavjud; real qurilmada o‘lchanmagan |
| Lighthouse Performance ≥90 | O‘lchanmagan, da’vo qilinmaydi |
| RSVP/ism/ota-ona/dress-code | RSVP/ism foydalanuvchi xohishiga ko‘ra yo‘q; qolganlari ixtiyoriy config |
| Avtomatik Telegram | Server kodi tayyor; secretlar/deployment yo‘q |

Botanik fon oldingi bosqichda built-in image generation orqali yaratilgan. Tavsif: “Cream ivory textured handmade paper with embossed white jasmine and small garden roses, delicate champagne gold botanical stems, floral arrangements lower left and upper right, empty middle for text, soft side light, no text, no people.” Bu bosqichda yangi AI surat yaratilmagan.


## Motion nashridagi qo‘shimchalar

Oltin shimmer matn, so‘zlarning mayin siljishi, ramkalarning suzishi/yaltirashi, surat ustidan nur o‘tishi, konvert muhrining to‘lqini, yurak va sana nuri, gulbarglar, tugmalardagi nur va monogramma shu’lasi. Hammasi CSS va kichik lokal skript bilan. WebGL zarrachalar soni ko‘paytirilmadi. Musiqa, manzil va Telegram mantig‘i o‘zgarmadi.


## Chizilgan fasad nashri

Foydalanuvchi so‘roviga ko‘ra ichki zal surati olib tashlandi va yangi tashqi ko‘rinish illustratsiyasi bilan almashtirildi. Tasvir haqiqiy fasadga asoslanmagan, sahifada “Fasadning badiiy talqini” deb belgilangan. Yangi bino tasviri built-in image generation yordamida yaratildi. Aniq prompt VENUE-ART.md faylida.
