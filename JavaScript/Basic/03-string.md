# String tipi (Matn) — nima, qayerdan kelgan, nega “string” deyiladi?

## 1) String nima?

**String** — bu odatda “matn”ni ifodalash uchun ishlatiladigan **belgilar ketma-ketligi** (sequence of characters/symbols). Kompyuter fanida “string” umumiy ma’noda “ketma-ket joylashgan elementlar” degan g‘oyani bildiradi, lekin amaliy dasturlashda ko‘pincha **matn belgilarining ketma-ketligi** nazarda tutiladi. :contentReference[oaicite:0]{index=0}

JavaScript’da `string`:

- **primitive** tip (oddiy qiymat),
- **immutable** (o‘zgarmas: ichidagi belgini joyida almashtirib bo‘lmaydi),
- amalda ko‘plab metodlarga ega ko‘rinadi, chunki engine kerak bo‘lganda uni **String wrapper object** bilan vaqtincha “o‘rab” beradi. :contentReference[oaicite:1]{index=1}

---

## 2) Nega “string” deb ataladi? (termin tarixi)

### 2.1 Oddiy til (English) manbasi

“String” ingliz tilida qadimdan **“ip”, “shnur”** degan ma’noda ishlatilgan. “Ipga tizilgan narsa” g‘oyasi keyin “ketma-ketlik” ma’nosiga ko‘chgan.

### 2.2 Ilmiy (matematika, mantiq, lingvistika) konteksti

Kompyuterdan oldin ham matematik mantiq va formal tillarda “string” atamasi **“ma’no berilmagan belgilar ketma-ketligi”** sifatida ishlatilgan. Keyin bu termin dasturlashga o‘tgan. :contentReference[oaicite:2]{index=2}

### 2.3 Dasturlash tarixidagi “string handling”

String bilan ishlash (pattern matching) g‘oyasi 1950–60’larda maxsus tillarda kuchli rivojlangan (masalan, COMIT, SNOBOL) — bu ham “string” terminining mustahkamlanishiga sabab bo‘lgan. :contentReference[oaicite:3]{index=3}

> Qisqa xulosa: “string” — “string of characters” (belgilar ipga tizilgandek ketma-ket) degan tasavvurdan chiqqan. :contentReference[oaicite:4]{index=4}

---

## 3) JavaScript’da String tipi qanday “ichki” ko‘rinishda?

ECMAScript spetsifikatsiyasiga ko‘ra, **String type** — bu **16-bit unsigned integer** qiymatlar ketma-ketligi bo‘lib, matn sifatida talqin qilinganda ular odatda **UTF-16 code unit** sifatida ko‘riladi. :contentReference[oaicite:5]{index=5}

Bu nimani anglatadi?

- JS string’ning `length`i ko‘pincha “harf soni” emas — **UTF-16 code unit** soni.
- Unicode’da ba’zi belgilar (masalan, 😄 kabi emoji) **2 ta code unit** bilan (surrogate pair) ifodalanadi. :contentReference[oaicite:6]{index=6}

### 3.1 Surrogate pair misoli (emoji)

```js
const s = '😄'
console.log(s.length) // 2  (ko‘pincha)
console.log(s.charCodeAt(0)) // birinchi 16-bit bo‘lak
console.log(s.charCodeAt(1)) // ikkinchi 16-bit bo‘lak
console.log(s.codePointAt(0)) // to'liq code point (emoji uchun to'g'riroq)
```

> Senior note: “length = harf soni” deb o‘ylash xato. Emoji/ba’zi iyerogliflarda ayniqsa.

---

## 4) String literal’lar: qanaqa yoziladi?

JS’da string literal:

- 'single quotes'

- "double quotes"

- `template literals`

```js
const a = 'salom'
const b = 'dunyo'
const name = 'Ali'
const c = `Hello, ${name}!` // interpolation
```

---

## 5) String immutable: bu nimani beradi?

Immutable degani — string ichidagi belgini joyida o‘zgartira olmaysan; har qanday “o‘zgartirish” yangi string yaratadi.

```js
let s = 'cat'
s[0] = 'b'
console.log(s) // "cat" (o‘zgarmaydi)

s = 'b' + s.slice(1)
console.log(s) // "bat"
```

String metodlari odatda yangi string qaytaradi (original o‘zgarmaydi).

---

## 6) “Primitive bo‘lsa, metodlar qayerdan keladi?”

**"hi".toUpperCase() ishlaydi** — lekin string primitive-ku?

JS engine shunday qiladi:

1. primitive string’da property/method kerak bo‘lsa

2. uni vaqtincha new String("hi") kabi wrapper bilan o‘rab

3. metodni shu wrapper’dan chaqiradi

Bu MDN’da aniq aytilgan.

```js
const x = "hello";
console.log(x.toUpperCase()); // "HELLO"

Senior note: new String("x") deyarli hech qachon ishlatma — u object bo‘lib qoladi va taqqoslash/true-false semantikasi chalkashadi.
```

---

## 7) Real hayot misollar (string bilan ishlash)

### 7.1 Input’ni tozalash (trim + normalizatsiya)

```js
function normalizeName(raw) {
	return raw.trim().replace(/\s+/g, ' ')
}

console.log(normalizeName('  Ali   Vali  ')) // "Ali Vali"
```

### 7.2 Xavfsiz substring (Unicode aware bo‘lish)

Agar “emoji ham bor” bo‘lsa, oddiy slice/length bilan ehtiyot bo‘lasan.

Eng sodda Unicode-aware yondashuv:

```js
const chars = Array.from('A😄B') // code point bo'yicha ajratadi
console.log(chars) // ["A","😄","B"]
console.log(chars.length) // 3
```

> Array.from(str) Unicode code point’larni yaxshiroq hisoblaydi (ko‘p holatda). (Bu umumiy JS xulq-atvori, lekin length code unit ekanini unutmang.)

### 7.3 URL query string yasash (amaliy)

```js
const params = new URLSearchParams({ q: 'js string', page: '1' })
console.log(params.toString()) // "q=js+string&page=1"
```

---

## 8) Eng ko‘p uchraydigan xatolar

| Xato                                          | Nega xato?                  | To‘g‘ri yondashuv                       |
| --------------------------------------------- | --------------------------- | --------------------------------------- |
| `str.length`ni “harf soni” deb olish          | UTF-16 code unit hisoblaydi | Unicode bo‘lsa `Array.from(str).length` |
| `new String("a")` ishlatish                   | object bo‘lib qoladi        | `String(x)` yoki literal                |
| Katta string’larni `+=` bilan loop’da yig‘ish | ko‘p new string yaratiladi  | array push + join (ba’zan)              |

---

### 9) Mini mashqlar

1. isPalindrome(str) yoz:

- bo‘sh joylarni olib tashla

- katta-kichik harfni farqlamasin

- “A man a plan a canal panama” kabi misolda ishlasin

2. maskPhone("+998901234567") -> "+99890**\***67" funksiyasi yoz.

- Emoji bor string’da “birinchi 2 ta belgi”ni kesib ol:

- slice bilan nima bo‘lishini ko‘r

- Array.from bilan to‘g‘ri yechim qil

---

## 10) Manbalar

1. MDN — String (primitive va wrapper): https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String

2. TC39 / ECMAScript spec — String UTF-16 code units: https://tc39.es/ecma262/

3. ECMA-262 2025 PDF (til tipi ta’rifi): https://ecma-international.org/wp-content/uploads/ECMA-262_16th_edition_june_2025.pdf

4. “String” atamasi tarixi va ta’rifi: https://en.wikipedia.org/wiki/String_%28computer_science%29
