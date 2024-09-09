# Chapter 2: 리액트 핵심 요소 깊게 살펴보기

## 2.3 클래스 컴포넌트와 함수 컴포넌트

### 2.3.1 클래스 컴포넌트
- 기존 리액트 16.8미만으로 작성된 코드는 클래서 컴포넌트 대다수
- 코드의 유지보수 내지는 오래전에 개발된 라이브러리 등 사용시 필요함으로 기본 클래스 컴포넌트 구조 이해할 필요 있다
\+ 사용방법 : 클래스 선언 > extends로 만들고 싶은 컴포넌트 extends하기
- constructor(): 컴포넌트 내부에 있다면 컴포넌트 가 초기화되는 시점에 호출된다.
\+ supur()는 컴포넌트를 만들면서 상송받은 상위 컴포넌트, 즉 React.Component의 생성자 함수를 먼저 호출해 필요한 상위 컴포넌트에 접근할 수 있게 도와준다.
- state : 클래스 컴포넌트 내부에서 관리하는 값을 의미, 이 값은 항상 객체여야 한다. 이 값을 변화가 있을때 마다 리렌더링 발생
- 메서드 : 렌더링 함수 내부에서 사용되는 함수, 보통 DOM에서 발생하는 이벤트와 함께 사용된다.
\+ 메서드 만드는 방식 크게 3가지: constructor에서 this 바인드 하는방법, 화살표 함수를 쓰는 방법, 렌더링 함수 내부에서 함수를 새롭게 만들어 전달하는 방법
(잘모르겠음)

#### 1. 클래스 컴포넌트의 생명주기 메서드
- 생명주기(life cycle)
\+ 마운트(mount)/ 업데이트(update)/ 언마운트(unmont)

+ render() : 유일한 필수 값, 마운트 업데이트 과정에서 일어남
\+ 항상 순수, 부수효과가 없어야 한다(no side-effects) 즉, 같은 입력값(props 또는 state)이 들어가면 항상 같은 결과물을 반환해야 한다
\+ state를 변경하는 일은, 클래스 컴포넌트의 메서드나 다른 생명주기 메서드 내부에서 발생해야 한다
+ componentDidMount() : 컴포넌트가 마운트되고 준비되는 즉시 실행
+ componentDidUpdate() : 컴포넌트 업데이트가 일어난 이후 바로 실행
+ componentWillUnmount() : 컴포넌트가 언마운트되거나, 더 이상 사용되지 않기 직전에 호출된다
\+ 이벤트를 지우거나, API 호출을 취소하거나, setInterval, setTimeout으로 생선된 타이머를 지우는 등의 작업을 하는 데 유용
<details>
<summary>예제</summary>
componentWillUnmount() {
    window.removeEventListener('resize', this.resizeListener)
    clearInterval(this.intervalId)
}
</details>

+ shouldComponentUpdate() : state나 props의 변경으로 리액트 컴포넌트가 다시 리렌더링되는 것을 막고싶은 경우 사용(특정한 서능 최적화 상황에서만 고려사용)
<details>
<summary>예제</summary>
shouldComponentUpdate(nextProps: Props, nextState: State) {
    // true인 경우, 즉 props의 title이 같지 않거나 state의 input이 같지 않은 경우에는
    // 컴포넌트를 업데이트한다. 이외의 경우에는 업데이트하지 않는다.
    return this.props.title !== nextProps.title || this.state.input !== nextState.input
}
</details>

\+ Component의 경우 버튼을 누르는 대로(state가 업데이트), 렌더링이 일어나지만 PureComponent는 state의 값이 업데이트되지 않아서 렌더링이 일어나지 않는다.
\+ pureComponent는 state값에 대해 얕은 비교를 수행해 결과가 드를 때만 렌더링 수행
\+ pureComponent는 얕은 비교만 수행하기 때문에 state객체와 같이 복잡한 구조의 데이터 변경은 감지 못하기 때문에 제대로 작동하지 않아, 성능에 역효과, 즉 적재적소 사용하기

+ static getDeriveStateFromProps() : render()를 호출하기 직전에 호출
+ getSnapShotBeforeUpdate() : DOM에 렌더링되기 전에 윈도우 크기를 조절하거나 스크롤 위치를 조정하느 등의 작업을 처리하는 유용한다.
+ getDerivedStateFromError() : 자식컴포넌트에서 에러 발생했을 때 호출되는 메서드