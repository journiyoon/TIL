# Array.prototype.reduce

reduce 메서드는 **자신을 호출한 배열의 모든 요소를 순회하며 하나의 결과값을 구해야** 하는 경우에 사용한다. 이 때 원본 배열은 **변경되지 않는다.**

# Try it

```js
const sum = [1, 2, 3, 4].reduce(
  (accumulator, currentValue, index, array) => accumulator + currentValue,
  0
);
console.log(sum); // 10
```

# Syntax

```js
array.reduce(callback, initialValue);
```

`reduce 메서드`는 첫 번째 인수로 콜백 함수, 두 번째 인수로 초기값을 전달받는다.<br>
`reduce 메서드의 콜백함수`에는 4개의 인수, 초기값 또는 콜백함수의 이전 반환값, reduce 메서드를 호출한 배열의 요소값, 인덱스, reduce 메서드를 호출한 배열 자체가 전달된다.

# Description

reduce 메서드는 초기값과 배열의 첫 번째 요소값을 콜백 함수에게 인수로 전달하면서 호출하고 다음 순회에는 콜백함수의 반환값과 두 번째 요소값을 콜백함수의 인수로 전달하면서 호출한다. 이러한 과정을 반복하여 **reduce 메서드는 하나의 결과값을 반환한다.**

# Examples

## 평균 구하기

```js
const values = [1, 2, 3, 4, 5];
const average = values.reduce((acc, cur, index, { length }) => {
  return index === length - 1 ? (acc + cur) / length : acc + cur;
}, 0);

console.log(average); // 3
```

> 원래 배열이 들어갈 자리에 {length}로 인수를 넣으면 배열의 길이를 사용할 수 있다.

## 최대값 구하기

```js
const values = [1, 2, 3, 4, 5];
const max = values.reduce((acc, cur) => {
  return acc > cur ? acc : cur;
}, 0);
console.log(max); // 5
```

> 최대값은 Math.max 메서드 활용이 더 직관적이고 간편하다.

```js
const values = [1, 2, 3, 4, 5];
const max = Math.max(...values);
OR;
const max = Math.max.apply(null, values);
console.log(max); // 5
```

## 요소의 중복 횟수 구하기

```js
const values = ["apple", "onion", "potato", "apple", "onion"];
const count = values.reduce((acc, cur) => {
  acc[cur] = (acc[cur] || 0) + 1;
  return acc;
}, {});
console.log(count); // {apple: 2, onion: 2, potato: 1}
/* 콜백함수 호출 과정
{apple: 1} => {apple: 1, onion: 1} => {apple: 1, onion: 1, potato: 1}
 => {apple: 2, onion: 1, potato: 1} => {apple: 2, onion: 2, potato: 1}
*/
```

처음 순회 시에 초기값인 acc는 {}가 되고, cur은 첫 번째 요소인 'apple'이 된다. 초기값으로 전달받은 빈 객체에 요소값인 cur을 'key'로, 요소의 개수를 'value'로 할당한다. 만약 'value'가 undefined라면(처음 등장) 'value'를 1로 초기화한다.

## 중첩 배열 평탄화

```js
const values = [1, [2, 3], [4, 5], 6, 7];
const flatten = values.reduce((acc, cur) => acc.concat(cur), []);
console.log(flatten); // [1,2,3,4,5,6,7]
```

> 중첩 배열 평탄화는 Array.prototype.flat 메서드 활용이 더 직관적이다.

```js
const values = [1, [2, 3], [4, 5], 6, 7];
const flatten = values.flat();
console.log(flatten); // [1,2,3,4,5,6,7]
// 중첩 깊이 값이 1이 아닌 경우
const values = [1, [2, 3], [4, [5]]];
const flatten = values.flat(2); // 인수로 중첩 깊이 값을 할당한다.
console.log(flatten); // [1,2,3,4,5]
```

## 중복 요소 제거

```js
const values = [1, 2, 1, 2, 3, 4, 5, 3, 3, 3, 4, 5];
const remove = values.reduce(
  (unique, value, index, _values) =>
    _values.indexOf(value) === index ? [...unique, value] : unique,
  []
);
console.log(remove); // [1,2,3,4,5]
```

> filter 메서드를 사용하는 방법이 더 직관적이다.

```js
const values = [1, 2, 1, 2, 3, 4, 5, 3, 3, 3, 4, 5];
const remove = values.filter(
  (value, index, _values) => _values.indexOf(value) === index
);
console.log(remove); // [1,2,3,4,5]
```

> Set을 사용하는 방법도 있다.

```js
const values = [1, 2, 1, 2, 3, 4, 5, 3, 3, 3, 4, 5];
const remove = [...new Set(values)];
console.log(remove); // [1,2,3,4,5]
```

# Warning

## 초기값을 꼭 써야하는 이유

**틀린 예제**

```js
const products = [
  { id: 1, price: 100 },
  { id: 2, price: 200 },
  { id: 3, price: 300 },
];
const priceSum = products.reduce((acc, cur) => acc.price + cur.price);
console.log(priceSum); // NaN
```

> 첫 번째 순회 시, acc = { id: 1, price: 100 }, cur = { id: 2, price: 200 }이다.<br>
> 두 번째 순회 시, acc = 300, cur = { id: 3, price: 300 }이다.<br>
> 두 번째 순회 시, acc에 숫자값이 전달되어 acc.price가 undefined가 된다.

**옳은 예제**

```js
const products = [
  { id: 1, price: 100 },
  { id: 2, price: 200 },
  { id: 3, price: 300 },
];
const priceSum = products.reduce((acc, cur) => acc + cur.price, 0);
console.log(priceSum); // 600
```
