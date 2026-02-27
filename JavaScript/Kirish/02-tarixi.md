# JavaScript tarixi (batafsil)

> JS tarixi — web tarixi bilan birga yuradi: brauzer urushlari, standartlashuv, engine’lar evolyutsiyasi va ekotizim portlashi.

## 1) Qisqa “elevator story”

- **1995**: Netscape’da Brendan Eich 10 kun ichida birinchi prototipni yaratadi (nomlari: Mocha → LiveScript → JavaScript).
- **1996–1997**: Netscape va Microsoft raqobati kuchayadi; standartlashuv uchun Ecma International (TC39) tuziladi; **ECMA-262 (ES1)** qabul qilinadi.
- **1999**: **ES3** — uzun vaqt “default” bo‘lib qolgan katta versiya.
- **2005–2010**: AJAX va web app’lar boom; **ES5 (2009)** web’ni modernlashtiradi.
- **2008–2009**: **V8** (Chrome) va **Node.js** paydo bo‘ladi → JS serverga chiqadi.
- **2015 (ES2015/ES6)**: JS zamonaviy tilga aylanadi (let/const, class, modules, arrow, promises…).
- **2016→hozir**: TC39 **yillik** release’lar bilan tilni doimiy rivojlantiradi.

---

## 2) Kim yaratgan?

### Brendan Eich (asosiy muallif)

Brendan Eich — Netscape’da ishlagan muhandis. U web sahifalarda “yengil skript” kerak bo‘lgani uchun til prototipini juda qisqa muddatda yaratadi.

### Netscape (kompaniya)

JavaScript’ning ilk “uy”i — **Netscape Navigator** brauzeri.

![Netscape Navigator era](https://commons.wikimedia.org/wiki/Special:FilePath/Netscape_logo.svg)

### Sun Microsystems (nomlash konteksti)

1995 yilda “Java” juda mashhur bo‘lgani uchun marketing/hamkorlik kontekstida til **JavaScript** deb nomlanadi (Java bilan sintaksis o‘xshash, lekin til arxitekturasi boshqa).

---

## 3) Nega JS yaratilgan?

1994–1995 yillarda web statik edi. Netscape maqsadi:

- forma validatsiya (client-side),
- oddiy interaktivlik,
- sahifani “jonlantirish”

uchun **yengil** va **tez** ishlovchi til yaratish bo‘lgan.

> Dastlab JS “oddiy” skript tili sifatida ko‘rilgan, lekin keyin web platforma bo‘lib ketdi.

---

## 4) Dastlabki nomlar: Mocha → LiveScript → JavaScript

| Davr       | Nom            | Izoh                     |
| ---------- | -------------- | ------------------------ |
| 1995 (May) | **Mocha**      | Ichki kod nom            |
| 1995 (Sep) | **LiveScript** | Netscape marketing nomi  |
| 1995 (Dec) | **JavaScript** | Netscape + Sun konteksti |

---

## 5) “Browser wars” va standartlashuv (Ecma / TC39)

1990’lar oxiri:

- Netscape’da JavaScript,
- Microsoft’da JScript (o‘xshash, lekin farqli implementatsiya)

Natija: “bir sayt bir brauzerda ishlaydi, boshqasida ishlamaydi” muammosi.

Shu sabab:

- 1996 yilda standartlashuv jarayoni boshlanadi
- **Ecma International** ichida **TC39** komiteti tilni standartlashtiradi
- 1997: **ECMA-262 (ES1)** qabul qilinadi

---

## 6) Versiyalar timeline (katta milestone’lar)

> Jadvalni yodlab olish shart emas, lekin “nega ES2015 katta burilish bo‘lganini” tushunish kerak.

| Yil   | Standart     | Muhim yangilik                                                |
| ----- | ------------ | ------------------------------------------------------------- |
| 1997  | ES1          | Birinchi standart (ECMA-262)                                  |
| 1998  | ES2          | ISO bilan moslashtirish (asosan editorial)                    |
| 1999  | ES3          | Regex, try/catch, yaxshilanishlar — uzoq vaqt dominant        |
| 2009  | ES5          | strict mode, JSON, array methods, Object.defineProperty       |
| 2015  | ES2015 (ES6) | let/const, class, modules, arrow, template literals, Promise… |
| 2016→ | ES2016+      | yillik release: incremental feature’lar                       |

---

## 7) ES4 nega “yo‘q”?

ES4 juda katta va murakkab reja bo‘lgan (types, katta sintaksis o‘zgarishlari va h.k.). Community va vendor’lar kelisha olmaydi.
Natija:

- ES4 bekor qilinadi
- keyin strategiya o‘zgaradi: “kichik-kichik, tez-tez release” (yillik) modeli.

> Senior fikr: katta “big-bang” o‘rniga “incremental shipping” web ekotizim uchun yaxshiroq ishladi.

---

## 8) Engine’lar evolyutsiyasi: V8 va performance

### 2008: Google Chrome + V8

V8 JavaScript’ni juda tezlashtirdi (JIT compilation, optimizatsiyalar). Bu “JS bilan katta web-app qilish mumkin” degan ishonchni kuchaytirdi.

### 2009: Node.js (server-side JS)

Node.js V8’ni olib, serverda event-loop asosida IO-heavy backend’lar yozishga imkon berdi.

> Shundan keyin “JS faqat brauzer tili” degan gap tugadi.

---

## 9) Real misollar: “old JS” vs “modern JS”

### 9.1 ES3/ES5 uslub (klassik)

```javascript
function User(name) {
	this.name = name
}
User.prototype.sayHi = function () {
	return 'Hi ' + this.name
}

var u = new User('Ali')
console.log(u.sayHi())
```

### 9.2 ES2015+ uslub (zamonaviy)

```js
class User {
	constructor(name) {
		this.name = name
	}
	sayHi() {
		return `Hi ${this.name}`
	}
}

const u = new User('Ali')
console.log(u.sayHi())
```

### 9.3 Async evolyutsiyasi: callback → promise → async/await

```js
// callback style (legacy)
readFile('a.txt', (err, data) => {
	if (err) return console.error(err)
	console.log(data)
})

// promise style
readFilePromise('a.txt').then(console.log).catch(console.error)

// async/await
;(async () => {
	try {
		const data = await readFilePromise('a.txt')
		console.log(data)
	} catch (e) {
		console.error(e)
	}
})()
```

---

## 10) Bugungi JS: TC39 jarayoni (senior ko‘z bilan)

TC39 feature’larni bosqichma-bosqich (stage 0–4) olib kiradi:

- proposal → discussion → spec detail → shipped

Bu model:

- vendor’lar kelishuvini,

- backwards compatibility’ni,

- ekotizim barqarorligini

ta’minlaydi.

---

## 11) Mini mashqlar

1. Timeline jadvalidan ES5 va ES2015 feature’larini ajratib yoz (10 tadan).

2. “Nega ES4 bekor bo‘ldi?” mavzusini 5–7 gapda o‘zing izohlab ber.

3. Old prototype misolini classga konvert qil.

---

## 12) Manbalar (primary & reliable)

- Brendan Eich haqida: https://en.wikipedia.org/wiki/Brendan_Eich

- ECMAScript tarixi (overview): https://en.wikipedia.org/wiki/ECMAScript

- ECMA-262 1st edition (June 1997 PDF): https://www.ecma-international.org/wp-content/uploads/ECMA-262_1st_edition_june_1997.pdf

- TC39 official spec (latest): https://tc39.es/ecma262/

- MDN — JavaScript (docs): https://developer.mozilla.org/en-US/docs/Web/JavaScript

---

### Faktlarni tekshirgan asosiy manbalar (sen yozayotgan tarix bo‘limi uchun)

- Mocha/LiveScript/JavaScript nomlash ketma-ketligi va 1995 yil konteksti: :contentReference[oaicite:0]{index=0}
- ECMA-262 1-edition June 1997 va standardlashuv boshlanishi: :contentReference[oaicite:1]{index=1}
- JS logosi (Wikimedia fayllari): :contentReference[oaicite:2]{index=2}

Agar xohlasang, keyingi bosqichda men:

- `README.md` uchun **TOC + learning path + progress checklist**,
- `JavaScript/Kirish/02-ishlash-muhiti.md` (browser vs node, devtools, strict mode),
- va `01-basics` bo‘limini shunday uslubda davom ettirib beraman.
  ::contentReference[oaicite:3]{index=3}
