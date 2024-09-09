# 리액트 핵심 요소 깊게 살펴보기

## 2.1 JSX란?

JSX는 페이스북이 만들었지만, 리액트용이라기 보다는 XML과 유사한 내장형 구문이며 독자적인 문법이다. 하지만 ECMAScript라 불리는 자바스크립트 표준의 일부는 아니다.

요약하면, JSX는 자바스크립트 내부에서 표현하기 까다로웠던 XML 스타일의 트리 구문을 작성하는 데 도움을 주는 새로운 문법으로 볼 수 있다.

### 2.1.1 JSX의 정의

기본적으로 `JSXElement`, `JSXAttributes`, `JSXChildren`, `JSXStrings`라는 4가지 컴포넌트를 기반으로 구성되어 있다.

#### 📌 JSXElement

HTML의 요소와 비슷한 역할을 한다. 아래는 JSXElement가 되기위한 조건이다.

1. `JSXOpeningElement`: 일반적인 요소이며, HTML 태그처럼 열린 태그가 있다면 닫힌 태그가 선언되어 있어야 한다.
2. `JSXClosingElement`: `JSXOpeningElement`와 쌍으로 사용되어야 한다.
3. `JSXSelfClosingElement`: 요소가 시작되고, 스스로 종료되는 HTML의 셀프 클로징과 같다.
4. `JSXFragment`: 아무 요소가 없는 형태로, `<></>`로 표현이 가능하다.

##### JSXElementName

JSXElement의 요소 이름으로 쓸 수 있는 것을 의미한다.

1. `JSXIdentifier`: 자바스크립트 식별자 규칙과 동일하다.

   ```jsx
   <$></$>
   <_></_>
   ```

2. `JSXNamespaceName`: `JSXIdentifier:JSXIdentifier`의 형태이며, :로 하나의 다른 식별자를 이어줄 수 있다.

   ```jsx
   <foo:bar></foo:bar>
   ```

3. `JSXMemberExpression`: `JSXIdentifier.JSXIdentifier`의 형태이며, 점 표기로 여러 개를 이을 수 있다.

   ```jsx
   <foo.bar></foo.bar>
   ```

#### 📌 JSXAttributes

`JSXElement`에 부여할 수 있는 속성을 의미한다. 필수값이 아니며, 존재하지 않아도 에러가 나지 않는다.

1. `JSXSpreadAttributes`: 자바스크립트의 전개 연산자와 동일한 역할을 한다.

   ```jsx
   <foo {...props} />
   ```

2. `JSXAttribute`: 속성을 나타내는 키와 값으로 짝을 이루어 표현한다.

   ```jsx
   <foo name="choi" />
   <foo attribute=<div>hello</div> />
   ```

   - 위에서 JSX를 값으로 보낼 때 `{}`를 하지 않았는데, 중괄호를 표기하는 것은 문법적인 오류가 나서가 아닌 React prettier의 규칙이다.

#### 📌 JSXChildren

JSXElement의 자식 값을 나타낸다.

1. `JSXChild`: 기본 단위이며, 필수값은 아니다. 일반적으로 알고있는 `children`과 같다.

#### 📌 JSXStrings

HTML에서 사용할 수 있는 문자열은 모두 사용하능하지만, JSX는 백슬래시(`\`)를 이스케이프 문자열로 처리하지 않는다.

### 2.1.2 JSX 예제 (생략)

### 2.1.3 JSX는 어떻게 자바스크립트에서 변환될까?

자바스크립트에서 JSX가 변환되는 방식을 알려면 React에서 JSX를 변환하는 `@babel/plugin-transform-react-jsx` 플러그인을 사용하면 트랜스파일한 코드를 볼 수 있다.

트랜스파일한 코드를 살펴보면, React.createElement가 사용된다는 사실을 알 수 있다.
