## 1.6 리액트에서 자주 사용하는 자바스크립트 문법

### 1.6.1 구조 분해 할당

#### 📌 배열 구조 분해 할당

대표적으로 `useState` 형태가 있다.

```js
const array = [1, 2, 3, 4, 5];
const [first, second, third, ...rest] = array;
// first 1
// second 2
// rest [4, 5]
```

#### 📌 객체 구조 분해 할당

```js
const object = {
  a: 1,
  b: 2,
};

const { a, b } = object;
// a 1
// b 2
```

### 1.6.2 전개 구문

#### 📌 배열의 전개 구문

```js
const arr1 = ["a", "b"];
const arr2 = [...arr1, "c", "d"]; // ["a", "b", "c", "d"]
```

#### 📌 객체의 전개 구문

```js
const obj1 = {
  a: 1,
  b: 2,
};

const obj2 = {
  c: 3,
  d: 4,
};

const newObj = { ...obj1, ...obj2 };
// { a: 1, b: 2, c: 3, d: 4}
```

배열은 트랜스파일 시 별 차이가 없는 반면에, 객체는 코드가 상당히 커진다. 이를 감안하여 사용해야 한다.

### 1.6.3 객체 초기자

```js
const a = 1;
const b = 2;

const obj = { a, b };
// { a: 1, b: 2 }
```

### 1.6.4 Array 프로토타입의 메서드: map, filter, reduce, forEach

#### 📌 Array.prototype.map

#### 📌 Array.prototype.filter

#### 📌 Array.prototype.reduce

#### 📌 Array.prototype.forEach

### 1.6.5 삼항 조건 연산자
