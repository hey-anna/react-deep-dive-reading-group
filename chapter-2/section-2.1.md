# Chapter 2: 리액트 핵심 요소 깊게 살펴보기

## 2.1 JSX란?
- 독자적인 문법
- ECMAScript이라 불리는 자바스크립트 표준의 일부는 "아니다"
- 페이스북이 만든 새로운 문법
- 트랜스파일러를 거쳐야 자바스크립트 코드로 변환된다(반드시)
- JSX는 자바스크립트 내부에서 표현하기 까다로웠던 XML 스타일의 트리 구문을 작성하는 데 많은 도움을 주는 새로운 문법이다(요약)

### 2.1.1 JSX의 정의
- JSXElement, JSXAttributes, JSXChildren, JSXSting 4가지 컴포넌트 기반으로 구성
#### 1. JSXElement
\* 컴포넌트 만들어 사용할 때 대문자로 시작(반드시) Why? HTML태그명과 사용자가 만든 컴포넌트 태그명을 구분 짓기 위해서이다
(미래에 추가되는 HTML에 대한 가능성을 열어두고 확실하게 구분할 수 있는 차이점을 두기 위한 것)
\+ JSXElementName : 숫자로 시작하거나 $와 _외의 다른 특수문자로 시작할 수 없다.
#### 2. JSXAttributes
#### 3. JSXChildren
- JSX는 속성을 가진 트리 구조를 나타내기 위해 만들어졌기 때문에 JSX로 부모와 자식 관계를 나타낼 수 있으며, 그 자식을 JSXChildren이라고 한다
- JSXChild JSXChildren을 이루는 기본 단위다.
- JSXChildren은 JSXChild를 0개 이상 가질 수 있다(없어도 상관없다)
#### 4. JSXSting
- HTML과 JSX 사이에 복사와 붙여넣기를 쉽게 할 수 있도록 설계되어 있다

### 2.1.3 JSX는 어떻게 자바스크립트에서 변환될까?
- 자바스크립트에서 JSX가 변환되는 방식을 알려면 리액트에서 JSX를 변환하는 @babel/plugin-transform-react-jax 플러그인을 알아야 한다
- JSXElement를 첫 번째 인수로 선언해 요소를 정의한다
- 옵셔널인 JSXChildren, JSXAttributes, JSXStrings는 이후 인수로 넘겨주어 처리한다
<details>
<summary>**JSX가 변환되는 특성 활용하기**</summary>
import {createElement} from 'react';

function TextOrHeading({
    isHeading,
    children,
}: PropsWithChildren<{ isHeading: boolean}>) {
    return createElement(
        isHeading ? 'h1' : 'span',
        {classNam: 'text'},
        children
    )
}
</details>

### 2.1.4 정리
- JSX는 자바스크립트 코드 내부에 HTML과 같은 트리 구조를 가진 컴포넌트를 표현할 수 있다는점에서 각광받고있다
- HTML문법과 자바스크립트가 섞여서 코드의 가독성을 해친다는 의견도 있다.
- 리액트 내부에서 JSX 변환 or 결과물 만드는지 알아두기(리액트 애플리케이션 만드는데 도움 됨)
- JSX 대부분 간결하게 컴포넌트를 구성하는게 효율적이나, 때에 따라 직접 createElement 사용해 컴포넌트를 구성하는 편이 효율적 일 수 있다
