# null va undefined — farqi, ichki mexanizmi va real hayotdagi muammolar

## 1) null va undefined nima?

JavaScript’da ikkalasi ham **“qiymat yo‘qligi”**ni bildiradi, lekin ma’nosi va kelib chiqishi boshqacha.

| Qiymat      | Turi      | Ma’nosi                         |
| ----------- | --------- | ------------------------------- |
| `undefined` | primitive | Qiymat hali berilmagan          |
| `null`      | primitive | Ataylab “bo‘sh” qilib qo‘yilgan |

```js
typeof undefined // "undefined"
typeof null // "object" ❗ (tarixiy bug)
```

---

## 2) undefined qachon paydo bo‘ladi?

### 2.1 O‘zgaruvchi e’lon qilinib, qiymat berilmasa

```js
let a
console.log(a) // undefined
```

### 2.2 Funksiya hech narsa qaytarmasa

```js
function f() {}
console.log(f()) // undefined
```

### 2.3 Mavjud bo‘lmagan property

```js
const user = { name: 'Ali' }
console.log(user.age) // undefined
```

### 2.4 Array index yo‘q bo‘lsa

```js
const arr = [1, 2]
console.log(arr[5]) // undefined
```

> Senior xulosa: `undefined` — bu ko‘pincha JS engine’ning o‘zi bergan default qiymat.

---

## 3) null qachon ishlatiladi?

`null` — bu developer tomonidan **ataylab “bo‘sh” qilish**.

```js
let currentUser = null
```

Masalan:

- hali login qilinmagan user
- hali yuklanmagan data
- DOM element topilmasa

```js
const el = document.querySelector('.unknown')
console.log(el) // null
```

> Senior mental model:
>
> - `undefined` = “yo‘q yoki berilmagan”
> - `null` = “ataylab bo‘sh”

---

## 4) Nega typeof null "object"?

Bu tarixiy xato.

JS 1995 yilda yaratilganda, qiymatlar ichki bit formatga ega edi. `null` object sifatida noto‘g‘ri belgilanib qolgan va backward compatibility sababli o‘zgartirilmagan.

```js
typeof null // "object"
```

> Senior interview savoli: “Bu bugmi?” — Ha, lekin intentional bug (legacy compatibility).

---

## 5) Taqqoslash: == vs ===

```js
null == undefined // true
null === undefined // false
```

Nega?

- `==` coercion qiladi
- `===` qat’iy tekshiradi

Muhim pattern:

```js
if (value == null) {
	// null yoki undefined bo‘lsa ishlaydi
}
```

Bu professional pattern hisoblanadi.

---

## 6) Truthy / Falsy

Ikkalasi ham falsy:

```js
Boolean(null) // false
Boolean(undefined) // false
```

---

## 7) Default qiymat berish

### 7.1 || bilan (ehtiyot!)

```js
let name = input || 'Guest'
```

Agar input `""` bo‘lsa ham "Guest" bo‘ladi.

### 7.2 ?? bilan (to‘g‘riroq)

```js
let name = input ?? 'Guest'
```

Bu faqat `null` va `undefined` uchun ishlaydi.

```js
'' ?? 'Guest' // ""
null ?? 'Guest' // "Guest"
```

> Senior rule: default qiymat uchun `??` ishlat.

---

## 8) Optional chaining (?.)

`null` yoki `undefined` bo‘lsa xatolik bermaydi:

```js
const user = null

console.log(user?.name) // undefined
```

Real hayot:

```js
if (response?.data?.user?.name) {
	console.log('User bor')
}
```

---

## 9) Real hayot muammolari

### 9.1 API response

```js
fetch('/api/user')
	.then(res => res.json())
	.then(data => {
		console.log(data.user.name)
	})
```

Agar `user` null bo‘lsa → error.

To‘g‘ri:

```js
console.log(data.user?.name ?? 'No name')
```

---

### 9.2 JSON bilan farq

JSON’da `undefined` yo‘q.

```js
JSON.stringify({ a: undefined })
// "{}"
```

```js
JSON.stringify({ a: null })
// {"a":null}
```

> Senior: API contract’da null ishlatiladi, undefined emas.

---

## 10) Engine darajasida farq

- `undefined` — global binding sifatida mavjud
- `null` — alohida primitive qiymat

ES spec’da ular alohida Type sifatida ko‘rsatiladi:

- Undefined type
- Null type

---

## 11) Eng ko‘p uchraydigan xatolar

| Xato                                   | Sabab         |                                              |              |
| -------------------------------------- | ------------- | -------------------------------------------- | ------------ |
| `typeof null === "object"` ga ishonish | tarixiy bug   |                                              |              |
| `                                      |               | ` bilan default berib 0 yoki "" ni yo‘qotish | falsy muammo |
| null check qilmasdan property o‘qish   | runtime error |                                              |              |

---

## 12) Professional patternlar

### 12.1 Nullish check

```js
if (value == null) {
	// null yoki undefined
}
```

### 12.2 Strict null check

```js
if (value === null) {
}
if (value === undefined) {
}
```

### 12.3 Safe access

```js
const name = user?.profile?.name ?? 'Anonymous'
```

---

## 13) Mini mashqlar

1. `isNil(value)` yoz:
   - null yoki undefined bo‘lsa true qaytarsin

2. `getUserName(user)` yoz:
   - user null bo‘lsa "Guest"

3. JSON.stringify’da undefined va null farqini sinab ko‘r

---

## 14) null vs undefined — qisqa xulosa

| Savol                     | Javob                       |
| ------------------------- | --------------------------- |
| Qiymat berilmaganmi?      | `undefined`                 |
| Ataylab bo‘shmi?          | `null`                      |
| Default fallback kerakmi? | `??` ishlat                 |
| JSON’da saqlanadimi?      | `null` ha, `undefined` yo‘q |

---

## 15) Manbalar

- MDN — null: [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/null](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/null)
- MDN — undefined: [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined)
- ECMAScript spec (Null & Undefined types): [https://tc39.es/ecma262/](https://tc39.es/ecma262/)
