### JavaScript — Kirish

<div style="display: flex; gap: 20px;">
  <img src="https://commons.wikimedia.org/wiki/Special:FilePath/JavaScript_unofficial_logo.svg" 
       width="150"
			 height="150" 
       alt="JavaScript logo">

<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSGdu4GErPdkG0PPM8UqivXJ0Dll4N0mFIRfBn4mdPWJtjYJxIepGILwI05bX_TKfMA6waWMpACukXAoPzSvXnNGKfI0Kx6XxT1DePHEmCrhw&s=10" 
       width="150" 
			 height="150"
       alt="Logo 2">

<img src="https://read.learnyard.com/content/images/size/w1600/2024/07/logo.png" 
       width="150" 
			 height="150"
       alt="Logo 3">

</div>

## 1) JavaScript nima?

**JavaScript (JS)** — web sahifalarni **interaktiv** qilish uchun yaratilgan, hozir esa:

- **brauzer** ichida (frontend),
- **server**da (Node.js, Deno, Bun),
- **mobil/desktop** (React Native, Electron),
- hatto **IoT** va **embedded** muhitlarda

ishlaydigan **umumiy maqsadli** dasturlash tilidir.

JavaScript’ni qisqa ta’riflasak:

> “JS — event-driven, single-threaded (lekin async orqali concurrency), prototypal object model’ga ega, dinamik tiplangan til.”

#### JS yordamida siz:

- tugma bosilganda reaksiya (event) qildirasiz,

- formani yuborishdan oldin tekshirasiz (validation),

- sahifada kontentni dinamik o‘zgartirasiz (DOM),

- serverdan ma’lumot olib kelib chiqarasiz (fetch / API),

- animatsiya, modal oynalar, dropdown, slider kabi UI’larni boshqarasiz.

---

## 2) JS qayerda ishlaydi?

### 2.1 Brauzer (Frontend)

Brauzer JS’ni ishlatib:

- DOM’ni o‘zgartiradi (UI update),
- event’larni ushlaydi (click, input, scroll),
- tarmoq so‘rovlarini yuboradi (fetch),
- local storage/cookie bilan ishlaydi.

### 2.2 Server (Backend)

Node.js kabi runtime’lar JS’ni serverda ishlatib:

- API yozish (REST/GraphQL),
- database bilan ishlash,
- real-time (WebSocket),
- background job’lar

qilishga imkon beradi.

---

## 3) JavaScript vs ECMAScript — farqi nima?

Ko‘pchilik chalkashtiradi:

| Atama               | Nimani anglatadi?                                                    | Misol                                           |
| ------------------- | -------------------------------------------------------------------- | ----------------------------------------------- |
| **ECMAScript (ES)** | Tilning **standarti** (spec). Syntax, types, object model va hokazo. | `let`, `class`, `Promise`                       |
| **JavaScript**      | ECMAScript + brauzer/host API’lari + ekotizim.                       | `document.querySelector`, `fetch`, `setTimeout` |
| **Engine**          | JS kodni bajaradigan **dvigatel**.                                   | V8 (Chrome/Node), SpiderMonkey (Firefox)        |
| **Runtime**         | Engine + API + event loop + IO imkoniyatlari.                        | Browser runtime, Node.js runtime                |

---

## 4) JS qanday ishlaydi (high-level)

JS odatda quyidagi modelda ishlaydi:

- **Call Stack** — sinxron kod ketma-ket bajariladi
- **Web/Host APIs** — timer, network, DOM event
- **Task queue / Microtask queue** — async callback’lar navbati
- **Event loop** — stack bo‘shaganda queue’dan ish olib keladi

> Shu sabab JS “single-threaded” bo‘lsa ham, async orqali juda ko‘p ishni “bloklamasdan” bajaradi.

---

## 5) Real misollar (Kirish darajasida)

### 5.1 Brauzerda interaktivlik (DOM)

```html
<button id="btn">Bos</button>
<p id="out"></p>

<script>
	document.addEventListener('DOMContentLoaded', function () {
		const btn = document.getElementById('btn')
		const out = document.getElementById('out')

		btn.addEventListener('click', function () {
			const time = new Date().toLocaleTimeString()
			out.textContent = 'Salom! Vaqt: ' + time
		})
	})
</script>
```

### 5.2 Serverda API (Node.js)

```javascript
// node:18+ (minimal demo)
import http from 'node:http'

const server = http.createServer((req, res) => {
	if (req.url === '/health') {
		res.writeHead(200, { 'content-type': 'application/json' })
		res.end(JSON.stringify({ ok: true, ts: Date.now() }))
		return
	}

	res.writeHead(404)
	res.end('Not Found')
})

server.listen(3000, () => console.log('http://localhost:3000'))
```

### 5.3 Async fikrlash (Promise/await)

```javascript
async function getUser() {
	// fetch brauzerda ham, node:18+ da ham bor
	const res = await fetch('https://jsonplaceholder.typicode.com/users/1')
	if (!res.ok) throw new Error('Network error: ' + res.status)

	const user = await res.json()
	return user
}

getUser().then(console.log).catch(console.error)
```

## 6) JS’ning kuchli tomonlari

- Web’da default til (eng katta ecosystem)

- Tez iteratsiya: prototip → production

- Full-stack: bitta til bilan frontend+backend

- Katta community & kutubxonalar: React, Vue, Node ecosystem

## 7) JS’ning “ehtiyot bo‘ladigan” joylari

| Masala              | Nima uchun muhim?                             | Senior eslatma                  |
| ------------------- | --------------------------------------------- | ------------------------------- |
| Type coercion       | `==` va avtomatik konvertatsiya bug chiqaradi | Default `===`                   |
| Mutable object      | reference orqali o‘zgarib ketadi              | immutability pattern            |
| Async mental model  | event loop tushunilmasa “kutilmagan” output   | microtask vs macrotask          |
| Package xavfsizligi | supply chain risk                             | lockfile + audit + minimal deps |

## 8) Kichik mashqlar

1. DOM misolini shunday qil: har bosganda count++ bo‘lsin.

2. /users/:id ga o‘xshash endpoint simulyatsiya qil (Node).

3. fetch misoliga timeout (AbortController) qo‘sh.

## 9) Manbalar

- MDN (JavaScript haqida): https://developer.mozilla.org/en-US/docs/Web/JavaScript

- JavaScript.info: https://javascript.info/

- ECMAScript spec (TC39): https://tc39.es/ecma262/

- ECMA-262 1st edition (1997 PDF): https://www.ecma-international.org/wp-content/uploads/ECMA-262_1st_edition_june_1997.pdf
