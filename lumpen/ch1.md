## 리액트에서의 동등 비교
자바스크립트의 동등 비교는 ==, ===, Object.is 다
Object.is 는 ES6 문법이기 떄문에 리액트에서는 Polyfill 을 함께 사용한다

#### Object.is()
```
Object.is() 는 두 개의 인수를 받아 동일한지 비교 후 반환하는 메서드로
비교하기 위해 하는 형변환을 하지 않는다
그렇지만 === (일치 연산자) 와는 다르게 조금 더 정교한 비교를 한다
```
```ts
-0 === +0 // true
Object.is(-0, +0) // false

Number.NaN === NaN // false
Object.is(Number.NaN, NaN) // true

NaN === 0/0 // false
Object.is(NaN, 0/0) // true
```
위와 같이 조금 더 개발자가 원하는 비교를 위해 내부적인 알고리즘이 동작한다
하지만 객체 비교는 얕은 비교를 한다


#### is()
런타임에 Object.is() 가 있으면 사용하고 아니라면 is() 를 사용한다
Object.is 는 익스플로러 등에 존재하지 않기 때문에 폴리필을 넣어준 것으로 보인다
```ts
function is(x: any, y: any) {
	return (
    	(x == y && (x !== 0 || 1/x === 1/y)) || (x !== x && y !== y) 
    )
}
```


#### objectIs()
objectIs 변수에는 Object.is() 또는 is() 함수가 들어간다
```ts
const objectIs: (x: any, y: any) => boolean =
      typeof Object.is === 'function' ? Object.is : is

export default objectIs
```

리액트에서는 objectIs() 를 기반으로 동등 비교를 하는
shallowEqual() 함수를 만들어 사용한다
의존성 비교 등 리액트의 동등 비교가 필요한 다양한 곳에서 사용된다

### shallowEqual() - 리액트의 비교 함수

shallowEqual() 의 내부 동작은
objectIs() 과 hasOwnProperty() 를 함께 사용하여
객체 간 비교도 추가되어 있다
다만 객체의 1뎁스만 비교하도록 구현되어 있다
객체의 크기는 무한정 될 수 있고
모든 비교를 재귀함수로 구현하면 성능상 이슈가 있을 것이기 때문에
1뎁스 까지만을 비교하는 것 같다
또한 기본적인 기능에서는 1뎁스 비교만 하면 충분하다
props 가 객체로 전달되기 때문에

때문에 **리액트 상태관리 시 객체타입의 데이터를 다루게 될 경우 주의**해야 한다
-> 필요한 곳에 깊은 비교 구현
또한 **props 는 객체**이기 때문에
**props 의 값에 객체를 사용하는 경우 더욱 주의**해야 하겠다



## 구조 분해 할당 (Destructuring assignment)

구조분해 할당 시 기본 값을 지정할 수 있는데
undefined 일 때만 기본 값을 사용하게 된다

### 객체 구조 분해 할당
객체 구조 분해 할당 시에는 key를 사용하는데
다른 이름으로 다시 할당 가능하다

객체 구조 분해 할당의 경우 ...rest 기능 사용 시
번들링 크기가 많이 커지므로 꼭 필요한지 고려 후
꼭 필요하다면 외부 라이브러리 등의 사용을 고려해보는 것도 좋다


## 전개 구문 (Spread Syntax)

배열의 전개 구문을 트랜스파일 하면 concat() 을 사용하였다
```ts
const arr1 = [1, 2]
const arr2 = [...arr1, 3, 4, 5]

var arr1 = [1, 2]
var arr2 = [].concat(arr1, [3, 4, 5])
```

### 객체 전개 구문
객체 전개 구문은 구조 분해 할당과 마찬가지로
...obj 사용 시 속성값 및 설명자 확인, 심벌 체크 등을 위해
트랜스파일된 코드가 매우 커지게 된다
사용하지 않는 편이 좋겠다


## 타입스크립트

### 타입 가드
타입스크립트 사용하는 쪽에서는 최대한 타입을 좁히는 것이 좋다
타입 가드를 사용하면 타입을 좁히는데 도움을 줄 수 있다

#### instanceof
incetanceof 는 지정한 인스턴스가 특정 클래스의 인스턴스인지 확인할 수 있다

```ts
if (e instanceof AxiosError) {}
```

#### typeof
typeof 는 특정 요소의 자료형을 확인한다

```ts
if (typeof value === 'string') {}
```

#### in
개체의 지정한 키가 존재하는지 확인한다
```ts
if ('property' in obejct) {}
```


### 덕타이핑
타입스크립트 핵심 원칙은 타입 검사가 값의 형태에 초점을 맞춘다는 것
객체의 타입이 클래스 상속, 인터페이스 구현 등으로 결정되는 것이 아니고
필요한 변수와 메서드만 가지고 있다면 해당 타입에 속하도록 인정해준다

Object.key() 타입 가드 함수

```ts
function keyOf<T extends Object>(obj: T): Array<keyof T> {
	return Array.from(Object.keys(obj)) as Array<keyof T>
}
```
