# 리액트 개발을 위해 꼭 알아야 할 자바스크립트

## 1.1 자바스크립트 동등 비교

React 훅에 들어가는 의존성 배열은 보통은 eslint의 도움을 받아 해당 배열을 채우곤 한다.(공감..)

React 컴포넌트 렌더링이 일어나는 이유 중 하나는 props의 동등 비교에 따른 결과다.(객체의 얕은 비교)

가상 DOM의 비교, 렌더링 판단, 메모이제이션 등의 모든 작업이 동등 비교를 기반으로 한다.

### 1.1.1 데이터 타입

---

#### 📌 원시 타입

`undefined`

선언 후 값을 할당하지 않으면 자동으로 할당되는 값이다.

`null`

`null`은 `object` 타입이며 초창기 자바스크립트의 문제 사항 중 하나이다.

```js
typeof null === "object"; // true
```

`null`을 판단할 때는 `null` 그 자체의 값을 활용해 판단할 수 있다.

```js
변수 === null;
```

객체와 배열은 항상 true로 취급되는 `truthy` 값이며, 유의해야 한다.

```js
const array = [];
if (array) {
  // true
}
```

`String`

문자열 타입

`Number`

숫자를 `integer`, `double` 등의 여러 타입으로 분리하는 다른 언어들과 달리, 자바스크립트는 모든 숫자를 `number` 타입 하나에 저장한다.

`BigInt`

기존 `number`는 `2^53 - 1`가 최대값이며, 그 이상의 값은 `BigInt`로 다룰 수 있다.

`Symbol`

심벌은 문자열이나 숫자와 다르게 심벌 함수를 이요해서만 만들 수 있다.

동일한 값을 사용하기 위해서는 `for` 메서드를 활용한다.

```js
const key = Symbol("key");
const key2 = Symbol("key");

key === key2; // false

Symbol.for("key") === Symbol.for("key");
```

#### 📌 객체 타입

원시 타입 이외의 모든 타입은 객체 타입이다.

값 자체가 아닌 참조를 전달한다고 해서 참조 타입이라고도 한다.

참조 타입은 같은 내용이라도 참조가 다르기 때문에 다르게 취급된다.

```js
const a = { name: "lee" };
const b = { name: "lee" };

a === b; // false
```

### 1.1.2 값을 저장하는 방식의 차이

---

원시 타입은 불변 형태의 값으로 저장되며, 할당 시점에 메모리 영역을 차지한다.

객체는 프로퍼티 삭제, 추가, 수정이 가능하므로 변경 가능한 형태로 저장되며, 값 자체의 복사가 아닌 참조를 전달한다.

### 1.1.3 자바스크립트의 또 다른 비교 공식, Object.is

---

일치 연산자(===)와 유사하지만, 좀 더 개발자가 기대하는(?) 방식으로 정확한 비교를 진행한다.

객체 비교에는 별 다른 차이가 없다.

```js
NaN === NaN; // false
Object.is(NaN, NaN); // true
```

### 1.1.4 리액트에서의 동등 비교

---

React에서 사용하는 비교는 `Object.is`이다.

`Object.is`는 ES6에서 제공하는 기능이기 때문에, React에서는 폴리필과 함께 사용한다. ([objectIs 코드](https://github.com/facebook/react/blob/1b7478246d05b030a2ae7a8bb07aea8c7df7ef27/packages/shared/objectIs.js#L11), [shallowEqual 코드](https://github.com/facebook/react/blob/1b7478246d05b030a2ae7a8bb07aea8c7df7ef27/packages/shared/shallowEqual.js#L18))

요약하자면 Object.is로 먼저 비교를 수행하고 이로 수행하지 못하는 비교, 즉 얕은 비교를 한 번 더 수행한다.

- React의 shallowEqual는 객체 1뎁스 까지는 비교가 가능하다.

- 2뎁스까지 가면 이를 비교할 방법이 없어 무조건 false를 반환한다.

얕은 비교까지만 구현한 이유는, props는 객체이고, 그 props 내용들만 일차적으로 비교하면 되기 때문이다.

<b>\*\* 만약, 상황에 따라 깊은 비교가 필요하다면 memo나, hooks의 의존성 배열을 활용해 적용하는 방법이 있다.</b>

### 1.1.5 정리

---

자바스크립트는 타 함수형 언어에 비해 객체 비교의 불완전성이라는 특성을 가지고 있고, 이는 기억해 두어야 한다.
