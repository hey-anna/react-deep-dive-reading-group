# Chapter 1: 리액트 개발을 위해 꼭 알아야 할 자바스크립트

## 1.4 클로저
- 함수형 프로그래밍의 패러다임을 이해하려면 클로저에 대해 반드시 알아야 한다.(필요성)

### 1.4.1 클로저의 정의
- 클로저는 함수와 함수가 선언된 어휘적 환경(Lexical Scope)의 조합
\* 선언된 어휘적 환경 : 변수가 코드 내부 어디에서 선언됐는지를 말하는 것
- 훅이 등장한 16.8 버전을 기점으로, 클로저 개념이 적극적 사용 되기 시작
- 코드가 작성된 순간에 정적으로 결정
\+ 호출방식에 따라 동적으로 결정되는 this 와 다르다.

### 1.4.2 변수의 유효 범위, 스코프
- 변수의 유효 범위에 따라서 어휘적 환경이 결정된다.
\* 스코프(scope) : 변수의 유효 범위
#### 1. 전역 스코프(global scope)
- 전역 레벨에서 선언하는 것
\+ 브라우저 환경에서 전역 객체 : window
\+ Node.js환경 : gloval
#### 2. 함수 스코프
- 다른언어와 달리 자바스크립트는 기본적으로 함수 레벨 스코프를 따른다. 즉, {} 블록이 스코프 범위를 경정하지 않는다.

### 1.4.3 클로저의 활용
- 선언된 함수 레벨 스코프를 활용해 어떤 작업을 할 수 있다는 개념이 바로 "클로저"
#### 1. 클로저의 활용
- 전역스코프 : 어디서든 원하는 값 꺼내 올 수 있다 = 반대로 누구든 접근 및 수정 할 수 있다
- 클로저의 개념을 활용하면 전역 스코프의 무분별한 사용을 막고, 개발자가 원하는 방향으로 노출 시킬 수 있다
#### 2. 리액트에서의 클로저
- useState
<details>
<summary>예제</summary>
function Component() {
    const [state, useState] = useState()

    function hadleClick() {
        // useState 호출은 위에서 끝났지만,
        // setState는 계속 내부의 최신값(prev)을 알고 있다.
        // 이는 클로저를 활용했기 때문에 가능하다
        setState((prev) => prev + 1)
    }
}
</details>

- 최신값 확인 How ? 클로저가 useState 내부에서 활용됐기 때문
- 외부 함수(useState)가 반환한 내부 함수(setState)는 외부함수 호출이 끝났어도,
자신이 선언된(저장 어딘가)곳을 기억하기때문에 state 값을 사용할 수 있다.

### 1.4.4 주의할 점
- var는 for문의 존재와 상관없이 해당 구문이 선언된 함수 레벨 스코프를 바라보고 있다,
함수 내부 실행이 아니면 전역 스코프에 var i가 등록되어 있다
- let은 기본적으로 블록 레벨 스코프를 가지게 되므로 let i가 for문을 순회하면서 각각의 스코프를 갖게된다.
setTimeout이 실행되는 시점에도 유효해서 각 콜백이 의도한 i 값을 바라본다.
\+ 66page 함수 참고
- 클로저의 개념, 즉 외부 함수를 기억하고, 이를 내부함수에서 가져다 쓰는 매커니즘은 성능에 영향미친다
꼭 필요한 작업이 아닌경우, 클로저 사용시 주의가 필요 = 클로저 사용은 적절한 스코프로 가둬두지 않는다면 성능에 악역향 미친다.(결론)

### 1.4.5 정리
- 부수 효과가 없고 순수해야 한다는 목적을 달성하기 위해 적극적으로 사용되는 개념이다.




#### 1. constructor(생성자)
- 객체를 생성하는 데사용하는 특수한 메서드(정의)
- 단 하나만 존재 할 수 있으며, 여러개 사용 시 에러 발생
- 별 다르게 수행할 작업이 없다면 생략 가능

#### 2. 프로퍼티
- 클래스로 인스턴스를 생성할 때 내부에 정의할 수 있는 속성값(정의)
<details>
<summary>예제</summary>
class Car {
    constructor(name) {
        // 값을 받으면 내부에 프로퍼티로 할당된다.
        this.name = name
    }
}

const myCar = new Car('자동차') // 프로퍼티 값을 넘겨주었다.
</details>

- 기본적으로 인스턴스 생성 시 constructor 내부에는 빈 객체가 할당돼 있는데,
이 빈 객체에 프로퍼티의 키와 값을 넣어서 활용할 수 있게 도와준다.
\* 접근 제한자 완벽지원 아니다. #을 붙여서 private 선언하는 방법 ES2019에 추가됐고, 또 타입스크립트를 활용하면 private, protected, public을 사용할 수 있다.
자바스크립트는 모든 프로퍼티가 public
과거 private가 없던 시절 _붙여 접근해서 안된다는 코딩 컨벤션이 있었지만, 기능적으로 private와 동일한 것은 아니다.

#### 3. getter와 setter
**getter 란 ?**
- 클래스에서 무언가 값을 가져올 때 사용
- get을 앞에 붙여, 뒤에는 getter 이름 선언해서 사용
<details>
<summary>예제</summary>
class Car {
    constructor(name) {
        this.name = name
    }
    get firstCharacter() {
    return this.name[0]
    }
}

const myCar = new Car('자동차')

myCar.firstCharacter // 차
</details>

**setter 란 ?**
- 클래스 필드에 값을 할당할 때 사용한다.
- getter와 동일하게 set 키워드를 먼저 선언 후, 뒤에 이름 붙여 사용

<details>
<summary>예제</summary>
class Car {
    constructor(name) {
        this.name = name
    }
    get firstCharacter() {
    return this.name[0]
    }

    set firstCharacter(char) {
        this.name = [char, ...this.name.slice(1).join('')]
    }
}

const myCar = new Car('자동차')

myCar.firstCharacter // 차

// '차'를 할당한다.
myCar.firstCharacter = '차'

console.log(myCar.firstCharacter, myCar.name) // 차, 자동차
</details>

#### 4. 인스턴스 메서드(=프로토타입메서드 : 자바스크립트의 prototype에 선언)
- 클래스 내부에서 선언한 메서드를 인스턴스 메서드라고 한다.
- 자바스크립트의 prototype에 선언되므로 
- Object.getPrototypeOf 와 __proto __ 동일하게 동작
\+ __proto __ 과거 호환성을 유지하기 위해 존재하기 때문에, 가급적 사용하지 않기
- 모든 객체는 프로토타입을 가지고 있는데, 특정 속성을 찾을때 자기 자신부터 시작해서 이 프로토 타입을 타고 최상위 객체인 Object까지 훑는다.
\* 프로토타입 체이닝 : 직접 객체에서 선언하지 않았음에도 프로토타입에 있는 메서드를 찾아서 실행을 도와주는 것
- 결론, 이 프로토타입과 프로토타입 체이닝이라는 특성 덕분에 생성한 객체에서도 직접 선언하지 않은, 클래스에 선언한 hello() 메서드를 호출할 수 있고, 이 메서드 내부에서 this도 접근해 사용할 수 있게 된다.

#### 4. 정적 메서드
- 객체를 생성하지 않더라도 여러 곳에서 재사용이 가능하다(장점)
so, 애플리케이션 전역에서 사용하는 유틸 함수를 정적 메서드로 많이 활용

#### 5. 상속(extends)
- 기존 클래스를 상속 받아서 자식클래스에서 이 상속받은 클래스를 기반으로 확장하는 개념이라 볼 수 있다
- Car를 extends한 Truck이 생성한 객체에서도, Truck이 따로 정의하지 않는 honk 메서드를 사용할 수 있다.
- 이 exgends를 활용하면 기본 클래스를 기반으로 다양한 파생된 클래스를 만들 수 있다
<details>
<summary>예제</summary>
class car {
    constructor(name) {
        this.name = name
    }

    honk() {
        console.log(`${this.name} 경적을 울립니다!`)
    }
}

class Truck extends Car {
    constructor(name) {
        // 부모 클래스의 constructor, 즉 Car의 constructor를 호출한다.
        super(name)
    }

    load() {
        console.log('짐을 싣습니다.')
    }
}

const myCar = new Car('자동차')
myCar.honk() // 자동차 경적을 올립니다!

const truck = new Truck('트럭')
truck.honk() // 트럭 경적을 울립니다.
truck.load() // 짐을 싣습니다!
</details>

### 1.3.2 클래스와 함수의 관계
- 클래스 작동하는 방식은 - 자바스크립트의 프로토타입을 활용하는 것
\+ 자바스크립트 클래스가 프로토타입을 기반으로 작동한다.
- 클래스는 객체지향 언어를 사용하던 다른 프로그래머가 좀 더 자바스크립트에 접근하기 쉽게 만들어주는 문법적 설탕 역할을 한다
<details>
<summary>예제</summary>
var Car = (function () {
    function Car(name) {
        this.name = name
    }

    // 프로토타입 메서드. 실제로 프로토타입에 할당해야 프로토타입 메서드로 작동한다.
    Car.prototype.honk = function () {
        console.log(`${this.name}이 경적을 울립니다`)
    }

    // 정적 메서드. 인스턴스 생성 없이 바로 호출 가능하므로 직접 할당했다.
    Car.hello = function () {
        console.log('저는 자동차 입니다')
    }

    // Car 객체에 속성을 직접 정의했다
    Object.defineProperty(Car, 'age', {
        // get과 set은 각각 접근자, 설정자로 사용할 수 있는 예약어다.
        // getter
        get : function () {
            return this.carAge
        },
        // setter
        set : function(value) {
            this.carAge = value
        },
    })

    return Car
})()
</details>


