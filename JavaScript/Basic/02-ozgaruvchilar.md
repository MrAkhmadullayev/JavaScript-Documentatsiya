# O‘zgaruvchilar (Variables) — `var`, `let`, `const`, scope va xotira modeli

![Memory diagram](https://commons.wikimedia.org/wiki/Special:FilePath/Call_stack_layout.png)

## 1) O‘zgaruvchi nima?

**O‘zgaruvchi** — bu qiymatni saqlash uchun berilgan **nom (identifier)**. JavaScript’da o‘zgaruvchi “quti” emas — u ko‘proq **label (yorliq)**: u **qiymatga** yoki **obyekt manziliga (reference)** bog‘lanadi.

JavaScriptda:

- primitive turlar odatda “qiymat” sifatida ko‘rinadi,
- object/function/array esa odatda “reference” orqali boshqariladi.

---

## 2) JS o‘zgaruvchilari qayerga “saqlanadi”?

Bu savolda 2 qatlam bor:

1. **Til darajasida (spec/konsept)**: o‘zgaruvchi — bu **lexical environment** (scope) ichidagi binding.
2. **Engine darajasida (V8, SpiderMonkey)**: engine buni optimizatsiya qilib, qiymatni:
   - stack’da,
   - heap’da,
   - yoki internal registry’da

saqlashi mumkin.

### 2.1 Konseptual model: Lexical Environment

Har scope (global/function/block) ichida engine **“environment record”** yaratadi va unda “nom → qiymat” bog‘lanishi turadi.

- `let`/`const` — **block scope** (`{ ... }` ichida yashaydi)
- `var` — **function scope** (block ichida bo‘lsa ham function ichida “ko‘rinadi”)

> Senior note: “qayerda saqlanadi?” degan savolga eng to‘g‘ri javob — “Scope’ning lexical environment’ida binding sifatida saqlanadi”. Stack/heap esa implementatsiya detali.

---

## 3) `var`, `let`, `const` farqi (eng muhim)

| Xususiyat             | `var`             | `let`              | `const`                   |
| --------------------- | ----------------- | ------------------ | ------------------------- |
| Scope                 | Function          | Block              | Block                     |
| Hoisting              | bor (`undefined`) | bor, lekin **TDZ** | bor, lekin **TDZ**        |
| Qayta e’lon qilish    | mumkin            | mumkin emas        | mumkin emas               |
| Qiymatni qayta assign | mumkin            | mumkin             | **mumkin emas** (binding) |

### 3.1 `var` hoisting

```js
console.log(a) // undefined
var a = 10
console.log(a) // 10
```

**Nima bo‘ldi?**

> **var a** deklaratsiyasi scope boshiga “ko‘tarildi”, lekin qiymat (**= 10**) keyin berildi.

## 3.2 let/const va TDZ (Temporal Dead Zone)

```js
console.log(x) // ReferenceError
let x = 5
```

Bu yerda **x** “hoisted” bo‘ladi, lekin initialization bo‘lmaguncha unga murojaat qilish mumkin emas.

## 4) Scope va shadowing (bir nomni “ustidan bosish”)

### 4.1 Block scope

```js
let x = 1

if (true) {
	let x = 2 // shadowing
	console.log(x) // 2
}

console.log(x) // 1
```

### 4.2 var block’ni “teshib” chiqadi

```js
if (true) {
	var y = 10
}
console.log(y) // 10 (function/global scope)
```

> Senior note: Kod bazada var ishlatmaslikning asosiy sababi — shunday “ko‘rinmas” buglar.

## 5) const nimani “o‘zgartirmaydi”?

**const** — bu binding o‘zgarmas degani. Obyektning ichidagi property’lar esa o‘zgarishi mumkin (object mutable).

```js
const user = { name: 'Ali' }

user.name = 'Vali' // OK (object ichini o‘zgartirdik)
// user = {} // TypeError (binding o‘zgarmaydi)
```

### 5.1 Object’ni haqiqatan “muzlatish”

```js
const settings = Object.freeze({ theme: 'dark' })

// strict mode’da:
settings.theme = 'light' // TypeError yoki silent fail (mode’ga bog‘liq)
```

## 6) Primitive vs Reference: o‘zgaruvchi aslida nimani ushlab turadi?

### 6.1 Primitive — qiymat “copy by value”

```js
let a = 'salom'
let b = a

b = 'hello'

console.log(a) // "salom"
console.log(b) // "hello"
```

### 6.2 Object — reference “copy by reference”

```js
let o1 = { count: 1 }
let o2 = o1

o2.count = 99

console.log(o1.count) // 99
console.log(o2.count) // 99
```

Tushuncha: o1 va o2 heap’dagi bitta obyektga “ko‘rsatkich” bo‘lib turibdi.

## 7) Xotira (Memory) bo‘yicha intuitiv model

Ko‘p engine’larda konseptual soddalashtirilgan model:

- Stack: function call’lar, local binding’lar, kichik/tez qiymatlar

- Heap: object/array/function (mutable strukturalar)

> Muhim: bu 100% qoidamas — engine optimizatsiyasi bor (escape analysis, inline caching, etc.). Lekin o‘rganish uchun yaxshi mental model.

### 7.1 Nega object heap’da bo‘ladi?

Chunki object:

- dinamik o‘lchamli,

- mutable,

- ko‘p reference bo‘lishi mumkin.

## 8) Global scope: “global object” va modul farqi

### 8.1 Brauzerda

Global scope ko‘pincha window bilan bog‘liq.

```js
var a = 1
console.log(window.a) // 1 (ko‘pincha)

let b = 2
console.log(window.b) // undefined (let global object property qilmaydi)
```

### 8.2 ES Modules (import/export)

ESM’da (module file) global scope “izolyatsiya” bo‘ladi va var ham windowga chiqmasligi mumkin.

> Senior note: Modern frontendlarda ESM default, shuning uchun “global” bilan ehtiyot bo‘l.

## 9) use strict va “global leak” xatolari

Strict bo‘lmasa:

```js
function f() {
	x = 10 // e'lon qilinmagan o'zgaruvchi -> global bo'lib ketishi mumkin
}
f()
console.log(x) // 10 (xavfli!)
```

Strict bo‘lsa:

```js
'use strict'
function f() {
	x = 10 // ReferenceError
}
```

ESM (import/export) fayllar default strict.

## 10) Amaliy patternlar (senior darajada)

### 10.1 Default: const, zarurat bo‘lsa let

```js
const users = []
let isLoading = true
```

### 10.2 Minimal scope prinsipi

O‘zgaruvchini kerak bo‘lgan joyga eng yaqin scope’da e’lon qil:

```js
function sumPositive(nums) {
	let total = 0

	for (const n of nums) {
		if (n > 0) total += n
	}

	return total
}
```

### 10.3 Immutability’ga moyillik

```js
const user = { name: 'Ali', age: 20 }

// mutation (tez, lekin ehtiyot):
// user.age++;

// immutable update (ko‘proq predictible):
const updated = { ...user, age: user.age + 1 }
```

## 11) Keng tarqalgan xatolar

1. var bilan loop + async:

```js
for (var i = 0; i < 3; i++) {
	setTimeout(() => console.log(i), 0)
}
// 3 3 3
```

To‘g‘risi:

```js
for (let i = 0; i < 3; i++) {
	setTimeout(() => console.log(i), 0)
}
// 0 1 2
```

2. const immutable deb o‘ylash (faqat binding).
3. TDZ’ni tushunmaslik:

```js
if (true) {
	// console.log(x); // ReferenceError
	let x = 1
}
```

## 12) Mini mashqlar (practice)

1. makeCounter() yoz:

- ichida let count = 0

- inc(), dec(), value() funksiyalari qaytsin

- closure ishlasin

2. var loop bug’ini 3 xil usul bilan fix qil:

- let

- IIFE (classic)

- setTimeout callback’ga argument berib

3. deepFreeze(obj) haqida o‘qi va kichik implementatsiya qilib ko‘r (advanced).

## 13) Manbalar

- MDN — let: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let

- MDN — const: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const

- MDN — var: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var

- JavaScript.info — Variables: https://javascript.info/variables

- JavaScript.info — Closures (keyin kerak bo‘ladi): https://javascript.info/closure
