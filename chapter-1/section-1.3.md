## 1.3 클래스

16.8 버전이 나오기 전까지 React에서는 클래스 컴포넌트만 존재했다.

### 1.3.1 클래스란 무엇인가?

특정한 객체를 만들기 위한 일종의 템플릿. 즉, 특정 형태의 객체를 반복적으로 만들기 위해 사용되는 것이 클래스다.

자바스크립트에서 클래스로 하는 모든 것들을 함수로도 동일하게 표현할 수 있다.

```js
class Car {
  constructor(name) {
    this.name = name;
  }

  honk() {
    console.log(this.name);
  }

  static hello() {
    console.log("hello");
  }

  set age(value) {
    this.carAge = value;
  }

  get age() {
    return this.carAge;
  }
}

const myCar = new Car("자동차");

myCar.honk(); // 메서드 호출
Car.hello(); // 정적 메서드는 직접 호출

myCar.age = 32; // setter로 값 할당
console.log(myCar.age); // 32
```

#### 📌 constructor

constructor는 생성자이며, 객체를 생성하는 데 사용하는 특수한 메서드다. 단 하나만 존재할 수 있다.

#### 📌 프로퍼티

인스턴스를 생성할 때 내부에 정의할 수 있는 속성값이다.

#### 📌 getter와 setter

값을 가져오고, 클래스 필드에 값을 할당할 때 사용한다.

#### 📌 인스턴스 메서드

클래스 내부에서 선언한 메서드를 인스턴스 메서드라고 하며, 실제로 자바스크립트의 prototype에 선언된다.

#### 📌 정적 메서드

인스턴스가 아닌 클래스 자체에서 직접 호출할 수 있는 메서드이며, this에 접근할 수 없다.

#### 📌 상속

기존 클래스를 상속받아 자식 클래스에서 상속받은 클래스를 기반으로 확장하는 개념이다.

```js
class Truck extends Car {
  constructor(name) {
    super(name); // 부모 클래스의 constructor
  }

  load() {
    console.log("짐을 싣습니다.");
  }
}

const myCar = new Car("자동차");
myCar.honk(); // 자동차
```

### 1.3.2 클래스와 함수의 관계

ES6 이전에는 프로토타입을 활용해 클래스 작동 방식을 직접 구현했고, 이는 클래스의 작동 방식이 자바스크립트의 프로토타입을 활용하는 것이라 볼 수 있다.
