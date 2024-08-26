# Chapter 1: 리액트 개발을 위해 꼭 알아야 할 자바스크립트

## 1.3 클래스
- 과거에 작성된 리액트 코드를 읽기 위해서, 코드를 함수 컴포넌트로 개선하기 위해서 어떤식으로 작동하는지 이해가 필요(필요성)

### 1.3.1 클래스란 무엇인가?
- 자바스크립트에서 클래스로 하는 모든 것들을 함수로도 동일하게 표현할 수 있다.
? 클래스 예제 훓, 이해잘 안감
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
#### 5. 정적 메서드
- 객체를 생성하지 않더라도 여러 곳에서 재사용이 가능하다(장점)
so, 애플리케이션 전역에서 사용하는 유틸 함수를 정적 메서드로 많이 활용
#### 6. 상속(extends)
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


