# For...in

`for...in`문은 **객체**에서 문자열로 키가 지정된 모든 열거 가능한 속성에 대해 반복합니다.

## Syntax

```md
for (variable in object) {
statements
}
```

## Examples

```js
const obj = { prodo: 10, muzi: 15, con: 20 };

for (const key in obj) {
  console.log(`${key}: ${obj[key]}`);
}

// expected output:
// "prodo: 10"
// "muzi: 15"
// "con: 20"
```

## Why Use

`객체의 반복`을 위해 만들어졌습니다. 객체의 키-값을 쉽게 확인할 수 있기 때문에 디버깅을 위해 사용할 수 있습니다.<br>
:warning: **warning:** 특정 순서에 따라 인덱스 반환하는 것을 보장하지 않기 때문에 `배열`에서는 `forEach` 또는 `for...of` 사용을 권장합니다.

### 참고

> :link: [공식문서](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/for...in)
