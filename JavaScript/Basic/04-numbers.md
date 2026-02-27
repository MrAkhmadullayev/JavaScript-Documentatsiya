# Number tipi — IEEE-754, NaN, Infinity, BigInt va amaliy jihatlar

## 1) Number nima?

JavaScript’da **Number** — bu **ikki xil sonni (butun va kasr)** bitta tipda ifodalaydigan primitive tur.

JS’da:

- `10` → integer
- `10.5` → floating point
- `-3.14`
- `Infinity`
- `NaN`

hammasi bitta `number` tipiga kiradi.

```js
typeof 10 // "number"
typeof 10.5 // "number"
typeof NaN // "number"
typeof Infinity // "number"
```

---

## 2) Ichki mexanizm: IEEE-754 (double precision)

JavaScript’dagi barcha number qiymatlar (BigInt’dan tashqari) IEEE-754 64-bit floating point formatda saqlanadi.

64 bit quyidagicha bo‘linadi:

| Qism                | Bit soni | Vazifasi      |
| ------------------- | -------- | ------------- |
| Sign                | 1 bit    | Musbat/manfiy |
| Exponent            | 11 bit   | Daraja        |
| Mantissa (Fraction) | 52 bit   | Kasr qismi    |

> Senior note: Shu sababli JS’da “butun son” degan alohida tip yo‘q — hammasi floating point.

---

## 3) Nega 0.1 + 0.2 ≠ 0.3 ?

Eng mashhur savol.

```js
0.1 + 0.2
// 0.30000000000000004
```

Sababi:

- 0.1 va 0.2 binary floating point’da aniq ifodalanmaydi

- yaqin qiymat bilan saqlanadi

- qo‘shilganda kichik xatolik paydo bo‘ladi

Tekshirib ko‘raylik:

```js
0.1 + 0.2 === 0.3
// false
```

To‘g‘ri taqqoslash (epsilon bilan):

```js
Math.abs(0.1 + 0.2 - 0.3) < Number.EPSILON
// true
```

> Production’da moliyaviy hisob-kitoblar uchun oddiy number emas, decimal kutubxona yoki integer cents modeli ishlatiladi.

## 4) Safe integer diapazoni

IEEE-754 64-bit number’da aniq ifodalanadigan butun sonlar chegaralangan:

```js
Number.MAX_SAFE_INTEGER // 9007199254740991
Number.MIN_SAFE_INTEGER // -9007199254740991
```

Agar bundan katta integer ishlatsang:

```js
9007199254740992 === 9007199254740993
// true ❗
```

Bu xavfli.

---

## 5) BigInt — katta butun sonlar uchun

Shu muammoni hal qilish uchun BigInt kiritilgan.

```js
const a = 9007199254740993n
const b = 2n

a + b // 9007199254740995n
typeof a // "bigint"
```

Muhim:

- number va bigint aralash ishlamaydi

```js
1n + 1
// TypeError
```

To‘g‘ri usul:

```js
BigInt(1) + 1n
```

> Senior note: BigInt moliya yoki kriptografiya uchun yaxshi, lekin performance va compatibility’ni hisobga olish kerak.

---

## 6) NaN (Not a Number)

NaN — noto‘g‘ri matematik operatsiya natijasi.

```js
Number('abc') // NaN
0 / 0 // NaN
```

Qiziq joyi:

```js
NaN === NaN // false ❗
```

To‘g‘ri tekshirish:

```js
Number.isNaN(NaN) // true
```

---

## 7) Infinity

```js
1 / 0 - // Infinity
	1 / 0 // -Infinity
```

Infinity bilan ham operatsiya mumkin:

```js
Infinity + 1 // Infinity
Infinity - Infinity // NaN
```

---

## 8) Number yaratish usullari

```js
const a = 10
const b = Number('10')
const c = parseInt('10px') // 10
const d = parseFloat('10.5') // 10.5
```

Farq:

```js
Number('10px') // NaN
parseInt('10px') // 10
```

> Senior note: parseInt’da radix ber:

```js
parseInt('10', 10)
```

---

## 9) Rounding (yaqinlashtirish)

```js
Math.round(4.6) // 5
Math.floor(4.9) // 4
Math.ceil(4.1) // 5
Math.trunc(4.9) // 4
```

2 xonali kasr:

```js
;(1.2345).toFixed(2) // "1.23" (string!)
```

---

## 10) Real hayot misollar

### 10.1 Pul hisoblash (xavfli usul)

```js
const price = 19.99
const tax = 0.2

const total = price * (1 + tax)
```

Bu rounding xatolik berishi mumkin.

### 10.2 To‘g‘riroq yondashuv (integer cents)

```js
const price = 1999 // cents
const tax = 20 // %

const total = price + (price * tax) / 100
console.log(total / 100)
```

---

## 11) Performance haqida

JS engine’lar integer ko‘rinadigan number’larni optimizatsiya qiladi (SMI — small integers).
Lekin agar number float’ga aylansa yoki tur aralashsa, optimizatsiya buzilishi mumkin.

> Senior note: Hot path’da tur barqarorligi muhim.

---

## 12) Eng ko‘p uchraydigan xatolar

| Xato                                         | Sabab               |
| -------------------------------------------- | ------------------- |
| 0.1 + 0.2 muammosi                           | IEEE-754            |
| MAX_SAFE_INTEGER’dan katta integer ishlatish | precision yo‘qoladi |
| NaN’ni `===` bilan tekshirish                | har doim false      |
| parseInt radix bermaslik                     | tarixiy muammo      |

---

## 13) Mini mashqlar

1. isSafeInteger(n) yoz:

- Number.isInteger

- Number.isSafeInteger

2. sumPrices(prices) yoz:

- floating muammosini oldini ol

- cents modeli bilan ishlat

3. 0.1 + 0.2 muammosini epsilon bilan yech.

---

## 14) Manbalar

1. MDN — Number: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number

2. MDN — BigInt: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt

3. ECMAScript spec (Number type): https://tc39.es/ecma262/

4. IEEE-754 floating point: https://en.wikipedia.org/wiki/IEEE_754
