# AccessBoard-Rang-Kontrasti-IndexedDB-offline-saqlash-bilan-
Bu bandlar ma'lumotlarni saqlash + accessibility talablarini bildiradi.

Oddiy qilib:

app/db.js — kartalar brauzerning IndexedDB bazasida saqlanadi.
createObjectStore — kartalarni saqlash uchun baza/jadval yaratadi.
put — yangi karta qo‘shadi yoki mavjud kartani yangilaydi.
getAll — barcha saqlangan kartalarni o‘qiydi.
Sahifa qayta ochilganda — kartalar yo‘qolib ketmaydi. IndexedDB'dan qayta o‘qilib, ekranga chiqariladi.

Holat faqat rang bilan ko‘rsatilmaydi — masalan, faqat yashil/qizil rang emas, balki:

✓ Bajarildi
⏳ Jarayonda
! Muhim

kabi matn yoki belgi ham bo‘ladi.

Kontrast kamida 4.5:1 — matn va fon orasidagi rang farqi yetarlicha aniq ekanini anglatadi. Bu ko‘rishida qiyinchiligi bo‘lgan foydalanuvchilar uchun ham o‘qishni yaxshilaydi.

Demak, bu talablar bajarilgan bo‘lsa, karta ma’lumotlari saqlanadi, refreshdan keyin tiklanadi va rangga bog‘liq bo‘lmagan accessibility ham ta’minlangan.

Senga buni 
kod orqali tekshirish kerakmi yoki 
hisobotga yoziladigan tayyor javob kerakmi?
