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
Ha, bu yerda muammo **loyiha ishlamagani emas**, asosan **GitHub repo ichida haqiqiy kod fayllari yo‘qligi** sababli 70/100 berilgan.

O‘qituvchi aynan shularni kutyapti:

### 1. `app/db.js` bo‘lishi kerak

Masalan:

```js
const DB_NAME = "AccessBoardDB";
const DB_VERSION = 1;
const STORE_NAME = "cards";

export function openDB() {
  return new Promise((resolve, reject) => {
    const request = indexedDB.open(DB_NAME, DB_VERSION);

    request.onupgradeneeded = () => {
      const db = request.result;

      if (!db.objectStoreNames.contains(STORE_NAME)) {
        db.createObjectStore(STORE_NAME, {
          keyPath: "id"
        });
      }
    };

    request.onsuccess = () => resolve(request.result);
    request.onerror = () => reject(request.error);
  });
}

export async function saveCard(card) {
  const db = await openDB();

  return new Promise((resolve, reject) => {
    const transaction = db.transaction(STORE_NAME, "readwrite");
    const store = transaction.objectStore(STORE_NAME);

    store.put(card);

    transaction.oncomplete = () => resolve();
    transaction.onerror = () => reject(transaction.error);
  });
}

export async function getCards() {
  const db = await openDB();

  return new Promise((resolve, reject) => {
    const transaction = db.transaction(STORE_NAME, "readonly");
    const store = transaction.objectStore(STORE_NAME);

    const request = store.getAll();

    request.onsuccess = () => resolve(request.result);
    request.onerror = () => reject(request.error);
  });
}
```

Bu yerda o‘qituvchi talab qilgan **`createObjectStore` + `put` + `getAll`** aniq ko‘rinadi.

### 2. Sahifa ochilganda kartalarni yuklash

Masalan, `app/main.js`:

```js
import { getCards, saveCard } from "./db.js";

async function loadBoard() {
  const cards = await getCards();
  renderBoard(cards);
}

loadBoard();
```

Shunda sahifa refresh qilinganda IndexedDB'dagi kartalar qayta chiqadi.

### 3. Rang + matn/belgi

Masalan:

```html
<span class="card-status status-done">
  ✓ Bajarildi
</span>

<span class="card-status status-progress">
  ⏳ Jarayonda
</span>

<span class="card-status status-todo">
  ○ Kutilmoqda
</span>
```

Bu yerda holat **faqat rang orqali emas**, matn va belgi orqali ham tushuniladi.

### 4. Kontrast

CSS'da ranglarni WCAG AA'ga mos qilib qo‘yish kerak:

```css
.card {
  background: #ffffff;
  color: #1a1a1a;
}

.card-status {
  font-weight: 700;
}

.status-done {
  color: #146c2e;
}

.status-progress {
  color: #7a4d00;
}

.status-todo {
  color: #444444;
}
```

Keyin ishlatilayotgan **har bir text/background kombinatsiyasini** kontrast checker'da tekshirib, kamida **4.5:1** ekanini tasdiqlash kerak.

### 5. Eng katta muammo — GitHub

O‘qituvchi:

> `README.md` bor, lekin `app/db.js` va boshqa haqiqiy kodlar yo‘q.

deb aytyapti.

Shuning uchun repo taxminan shunday ko‘rinishi kerak:

```text
AccessBoard/
│
├── index.html
├── README.md
│
├── app/
│   ├── db.js
│   ├── main.js
│   ├── render.js
│   └── styles.css
│
├── server/
│   └── server.js
│
├── package.json
└── .gitignore
```

Va bularning hammasini GitHub'ga **commit + push** qilish kerak.

**70 → 75+ olish uchun eng muhim ish:** `README.md`da kodni tasvirlash emas, **haqiqiy `app/db.js`, JavaScript, HTML va CSS fayllarini repo'ga joylash**.Ha, bu yerda muammo **loyiha ishlamagani emas**, asosan **GitHub repo ichida haqiqiy kod fayllari yo‘qligi** sababli 70/100 berilgan.

O‘qituvchi aynan shularni kutyapti:

### 1. `app/db.js` bo‘lishi kerak

Masalan:

```js
const DB_NAME = "AccessBoardDB";
const DB_VERSION = 1;
const STORE_NAME = "cards";

export function openDB() {
  return new Promise((resolve, reject) => {
    const request = indexedDB.open(DB_NAME, DB_VERSION);

    request.onupgradeneeded = () => {
      const db = request.result;

      if (!db.objectStoreNames.contains(STORE_NAME)) {
        db.createObjectStore(STORE_NAME, {
          keyPath: "id"
        });
      }
    };

    request.onsuccess = () => resolve(request.result);
    request.onerror = () => reject(request.error);
  });
}

export async function saveCard(card) {
  const db = await openDB();

  return new Promise((resolve, reject) => {
    const transaction = db.transaction(STORE_NAME, "readwrite");
    const store = transaction.objectStore(STORE_NAME);

    store.put(card);

    transaction.oncomplete = () => resolve();
    transaction.onerror = () => reject(transaction.error);
  });
}

export async function getCards() {
  const db = await openDB();

  return new Promise((resolve, reject) => {
    const transaction = db.transaction(STORE_NAME, "readonly");
    const store = transaction.objectStore(STORE_NAME);

    const request = store.getAll();

    request.onsuccess = () => resolve(request.result);
    request.onerror = () => reject(request.error);
  });
}
```

Bu yerda o‘qituvchi talab qilgan **`createObjectStore` + `put` + `getAll`** aniq ko‘rinadi.

### 2. Sahifa ochilganda kartalarni yuklash

Masalan, `app/main.js`:

```js
import { getCards, saveCard } from "./db.js";

async function loadBoard() {
  const cards = await getCards();
  renderBoard(cards);
}

loadBoard();
```

Shunda sahifa refresh qilinganda IndexedDB'dagi kartalar qayta chiqadi.

### 3. Rang + matn/belgi

Masalan:

```html
<span class="card-status status-done">
  ✓ Bajarildi
</span>

<span class="card-status status-progress">
  ⏳ Jarayonda
</span>

<span class="card-status status-todo">
  ○ Kutilmoqda
</span>
```

Bu yerda holat **faqat rang orqali emas**, matn va belgi orqali ham tushuniladi.

### 4. Kontrast

CSS'da ranglarni WCAG AA'ga mos qilib qo‘yish kerak:

```css
.card {
  background: #ffffff;
  color: #1a1a1a;
}

.card-status {
  font-weight: 700;
}

.status-done {
  color: #146c2e;
}

.status-progress {
  color: #7a4d00;
}

.status-todo {
  color: #444444;
}
```

Keyin ishlatilayotgan **har bir text/background kombinatsiyasini** kontrast checker'da tekshirib, kamida **4.5:1** ekanini tasdiqlash kerak.

### 5. Eng katta muammo — GitHub

O‘qituvchi:

> `README.md` bor, lekin `app/db.js` va boshqa haqiqiy kodlar yo‘q.

deb aytyapti.

Shuning uchun repo taxminan shunday ko‘rinishi kerak:

```text
AccessBoard/
│
├── index.html
├── README.md
│
├── app/
│   ├── db.js
│   ├── main.js
│   ├── render.js
│   └── styles.css
│
├── server/
│   └── server.js
│
├── package.json
└── .gitignore
```

Va bularning hammasini GitHub'ga **commit + push** qilish kerak.

**70 → 75+ olish uchun eng muhim ish:** `README.md`da kodni tasvirlash emas, **haqiqiy `app/db.js`, JavaScript, HTML va CSS fayllarini repo'ga joylash**.

Agar xohlasang, men senga **AccessBoard uchun to‘liq tayyor `index.html + app/db.js + app/main.js + CSS + server.js` strukturani** bitta qilib yozib beraman.


Agar xohlasang, men senga **AccessBoard uchun to‘liq tayyor `index.html + app/db.js + app/main.js + CSS + server.js` strukturani** bitta qilib yozib beraman.
