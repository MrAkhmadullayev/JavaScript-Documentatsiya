# Boolean tipi — true/false, truthy/falsy va mantiqiy fikrlash

## 1) Boolean nima?

**Boolean** — bu faqat **ikki qiymatga ega** bo‘lgan primitive tip:

```js
true
false
```

U nomini ingliz matematigi **George Boole** dan olgan. U 19-asrda mantiqiy algebra (Boolean algebra) asoslarini yaratgan. Zamonaviy kompyuter tizimlaridagi mantiqiy operatsiyalar aynan shu algebra asosida ishlaydi.

```js
typeof true // "boolean"
typeof false // "boolean"
```

---

## 2) Boolean qayerda ishlatiladi?

Boolean:

- `if` shartlarida
- loop’larda
- mantiqiy tekshiruvlarda
- flag (holat) sifatida
- validatsiyada

Misol:

```js
let isLoggedIn = true

if (isLoggedIn) {
	console.log('Dashboard ochildi')
}
```

---

## 3) Boolean qanday hosil bo‘ladi?

### 3.1 Taqqoslash operatorlari orqali

```js
5 > 3 // true
10 === 10 // true
'5' === 5 // false
```

### 3.2 Boolean konstruktori orqali

```js
Boolean(1) // true
Boolean(0) // false
Boolean('') // false
Boolean('hi') // true
```

Yoki qisqa usul:

```js
!!'hello' // true
!!0 // false
```

> Senior note: `!!value` — tez-tez ishlatiladigan boolean cast pattern.

---

## 4) Truthy va Falsy

JS’da boolean bo‘lmagan qiymatlar ham shart kontekstida `true` yoki `false` ga aylanadi.

### 4.1 Falsy qiymatlar (faqat 7 ta)

```js
false
0 - 0
0n
;('')
null
undefined
NaN
```

Qolgan hamma qiymatlar **truthy**.

### 4.2 Misollar

```js
if ('hello') {
	console.log('Bu ishlaydi') // ishlaydi
}

if (0) {
	console.log('Bu ishlamaydi')
}
```

---

## 5) Mantiqiy operatorlar (AND, OR, NOT)

### 5.1 AND (`&&`)

```js
true && true // true
true && false // false
```

Muhim: JS’da `&&` boolean emas, operand qaytaradi.

```js
'hello' && 123 // 123
0 && 'hi' // 0
```

### 5.2 OR (`||`)

```js
false || true // true
```

```js
'' || 'default' // "default"
```

> Senior note: `||` default value uchun ishlatiladi, lekin `0` yoki `""` ni ham falsy deb oladi.

### 5.3 Nullish coalescing (`??`)

`null` va `undefined` uchungina fallback qiladi:

```js
0 ?? 100 // 0
null ?? 100 // 100
undefined ?? 50 // 50
```

Bu `||` dan xavfsizroq.

---

## 6) Short-circuit behavior (juda muhim)

JS’da `&&` va `||` qisqa tekshiradi.

```js
true || console.log('ishlamaydi')
false && console.log('ishlamaydi')
```

Amaliy pattern:

```js
isLoggedIn && openDashboard()
```

---

## 7) Boolean object vs primitive

```js
const a = true
const b = new Boolean(true)

typeof a // "boolean"
typeof b // "object"
```

Yomon misol:

```js
if (new Boolean(false)) {
	console.log('Bu ishlaydi 😱')
}
```

Chunki object har doim truthy.

> Senior rule: `new Boolean()` ishlatma.

---

## 8) Taqqoslashda ehtiyot bo‘lish

### 8.1 `==` vs `===`

```js
0 == false // true
0 === false // false
```

`==` coercion qiladi.

> Senior rule: default `===` ishlat.

---

## 9) Real hayot misollar

### 9.1 Form validation

```js
function isValidEmail(email) {
	return email.includes('@') && email.length > 5
}
```

### 9.2 Feature flag

```js
const ENABLE_NEW_UI = true

if (ENABLE_NEW_UI) {
	renderNewUI()
}
```

### 9.3 Permission check

```js
function canEdit(user) {
	return user.role === 'admin' || user.role === 'editor'
}
```

---

## 10) Boolean algebra (asosiy qonunlar)

| Qonun           | Misol                     |     |                 |
| --------------- | ------------------------- | --- | --------------- |
| NOT             | `!true === false`         |     |                 |
| AND             | `true && false === false` |     |                 |
| OR              | `true                     |     | false === true` |
| Double negation | `!!value`                 |     |                 |

---

## 11) Performance haqida

Engine’lar boolean operatsiyalarni juda tez bajaradi.
Lekin shart ichida murakkab expression’lar yozish:

- readability’ni pasaytiradi
- bug ehtimolini oshiradi

Senior tavsiya:

- murakkab shartni alohida variable’ga ajrat.

```js
const isAdultUser = age >= 18 && isVerified

if (isAdultUser) {
	allowAccess()
}
```

---

## 12) Eng ko‘p uchraydigan xatolar

| Xato                           | Sabab         |                                      |              |
| ------------------------------ | ------------- | ------------------------------------ | ------------ |
| `new Boolean(false)` ishlatish | object truthy |                                      |              |
| `                              |               | ` bilan default berib 0 ni yo‘qotish | falsy muammo |
| `==` ishlatish                 | coercion      |                                      |              |

---

## 13) Mini mashqlar

1. `isTruthy(value)` yoz:
   - `!!value` qaytarsin

2. `isEmpty(value)` yoz:
   - `null`, `undefined`, `""`, `"   "` bo‘lsa false qaytarsin

3. `canAccess(user)` yoz:
   - user.active true
   - user.role "admin" yoki "moderator"

---

## 14) Manbalar

- MDN — Boolean: [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)
- ECMAScript spec — Boolean type: [https://tc39.es/ecma262/](https://tc39.es/ecma262/)
- Boolean algebra (George Boole): [https://en.wikipedia.org/wiki/Boolean_algebra](https://en.wikipedia.org/wiki/Boolean_algebra)
