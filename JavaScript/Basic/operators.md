# Operators — JavaScript operatorlari (to‘liq qo‘llanma)

> Fayl: `JavaScript/Basic/operators.md`

---

## 1) Operator nima?

**Operator** — bu qiymatlar (operandlar) ustida amal bajaradigan sintaktik element.

```js
5 + 3
// 5 va 3 — operand
// + — operator
```

JS’da operatorlar:

- matematik
- taqqoslash
- mantiqiy
- assignment
- bitwise
- string
- optional chaining
- nullish
- type operatorlari
- spread/rest
- destructuring
- va boshqalar

---

# 2) Arithmetic operatorlar

| Operator | Ma’nosi      |
| -------- | ------------ |
| +        | Qo‘shish     |
| -        | Ayirish      |
| \*       | Ko‘paytirish |
| /        | Bo‘lish      |
| %        | Qoldiq       |
| \*\*     | Daraja       |

```js
10 + 5 // 15
10 % 3 // 1
2 ** 3 // 8
```

### 2.1 + operatorining ikki xil xulqi

```js
5 + 5 // 10
'5' + 5 // "55"
```

> Senior rule: `+` string bilan ishlasa concatenation qiladi.

---

# 3) Assignment operatorlari

| Operator | Misol     |
| -------- | --------- |
| =        | x = 5     |
| +=       | x += 2    |
| -=       | x -= 1    |
| \*=      | x \*= 3   |
| /=       | x /= 2    |
| %=       | x %= 2    |
| \*\*=    | x \*\*= 2 |

```js
let x = 10
x += 5 // 15
```

---

# 4) Comparison operatorlari

| Operator | Ma’nosi               |
| -------- | --------------------- |
| ==       | Teng (coercion bilan) |
| ===      | Qat’iy teng           |
| !=       | Teng emas             |
| !==      | Qat’iy teng emas      |
| >        | Katta                 |
| <        | Kichik                |
| >=       | Katta yoki teng       |
| <=       | Kichik yoki teng      |

### 4.1 == vs ===

```js
0 == false // true
0 === false // false
```

> Senior rule: har doim `===` ishlat (faqat maxsus null-check holatlaridan tashqari).

---

# 5) Logical operatorlar

## 5.1 AND (&&)

```js
true && false // false
```

Short-circuit:

```js
isLoggedIn && openDashboard()
```

## 5.2 OR (||)

```js
false || true // true
```

Default pattern:

```js
let name = input || 'Guest'
```

## 5.3 Nullish (??)

```js
null ?? 'default' // "default"
undefined ?? 'default' // "default"
0 ?? 100 // 0
```

> Senior rule: default qiymat uchun `??` xavfsizroq.

---

# 6) Unary operatorlar

| Operator | Ma’nosi             |
| -------- | ------------------- |
| +x       | Number conversion   |
| -x       | Manfiy              |
| !x       | NOT                 |
| typeof   | Tipni aniqlash      |
| delete   | Property o‘chirish  |
| void     | undefined qaytaradi |

```js
;+'5' // 5
typeof 10 // "number"
delete obj.key
```

---

# 7) Increment / Decrement

```js
let x = 5
x++ // 6
++x // 7
```

Farqi:

```js
let a = 5
console.log(a++) // 5
console.log(++a) // 7
```

> Senior note: production’da `++` kam ishlatiladi, `a += 1` ko‘proq aniq.

---

# 8) Ternary operator

```js
condition ? value1 : value2
```

Misol:

```js
let age = 20
let status = age >= 18 ? 'adult' : 'minor'
```

> Murakkab ternary yozish readability’ni buzadi.

---

# 9) Optional chaining (?.)

```js
user?.profile?.name
```

Agar oraliq null/undefined bo‘lsa xatolik bermaydi.

---

# 10) Spread / Rest operator (...)

## 10.1 Spread

```js
const arr = [1, 2, 3]
const copy = [...arr]
```

Object:

```js
const obj = { a: 1 }
const newObj = { ...obj, b: 2 }
```

## 10.2 Rest

```js
function sum(...nums) {
	return nums.reduce((a, b) => a + b, 0)
}
```

---

# 11) Destructuring

```js
const user = { name: 'Ali', age: 20 }
const { name, age } = user
```

Array:

```js
const [a, b] = [1, 2]
```

---

# 12) in operatori

```js
'name' in user
```

Property mavjudligini tekshiradi.

---

# 13) instanceof

```js
;[] instanceof Array // true
```

Prototype chain asosida ishlaydi.

---

# 14) Bitwise operatorlar

| Operator | Ma’nosi     |
| -------- | ----------- | --- |
| &        | AND         |
|          |             | OR  |
| ^        | XOR         |
| ~        | NOT         |
| <<       | Left shift  |
| >>       | Right shift |

```js
5 & 1 // 1
```

> Ko‘p ishlatilmaydi, lekin performance-sensitive joylarda yoki low-level manipulyatsiyada kerak bo‘ladi.

---

# 15) Operator precedence (ustuvorlik)

Misol:

```js
2 + 3 * 4 // 14
```

Chunki `*` ustuvor.

Qavs ishlatish yaxshi amaliyot:

```js
;(2 + 3) * 4 // 20
```

> Senior rule: murakkab expression’larda qavs bilan aniq yoz.

---

# 16) Real hayot patternlari

## 16.1 Safe default

```js
const limit = config.limit ?? 10
```

## 16.2 Guard pattern

```js
isValid && save()
```

## 16.3 Immutable update

```js
const updated = { ...user, age: user.age + 1 }
```

---

# 17) Eng ko‘p uchraydigan xatolar

| Xato                             | Sabab       |                      |       |
| -------------------------------- | ----------- | -------------------- | ----- |
| == ishlatish                     | coercion    |                      |       |
|                                  |             | bilan 0 ni yo‘qotish | falsy |
| ++ ni noto‘g‘ri joyda ishlatish  | side-effect |                      |       |
| Bitwise va logical aralashtirish | chalkashlik |                      |       |

---

# 18) Mini mashqlar

1. `safeDivide(a,b)` yoz:
   - b 0 bo‘lsa null qaytarsin

2. `getDisplayName(user)`:
   - user.name ?? "Anonymous"

3. Spread bilan array clone qil va mutation tekshir.

---

# 19) Senior xulosa

- Har doim `===`
- Default uchun `??`
- Murakkab shartni alohida variable’ga ajrat
- Immutability uchun spread
- Bitwise operatorlarni faqat kerak bo‘lsa ishlat

---

# 20) Manbalar

- MDN Operators: [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators)
- ECMAScript spec: [https://tc39.es/ecma262/](https://tc39.es/ecma262/)
