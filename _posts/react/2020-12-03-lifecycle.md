---
title: "React LifeCycle"
layout: post
categories:
  - react
tags:
  - concept
  - lifecycle
last modified_at: 2020-12-03T23:07:24
---

컴포넌트가 DOM에 생성되기 전후, 상태 업데이트 전후 등 각 생명주기별로 로직을 넣을 수 있음.  
클래스형 컴포넌트에서 사용됨.

[![LifeCycle](/images/react/react-lifecycle.png)](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

#### constructor

```javascript
constructor(props) {
  super(props);  // 맨처음 선언 필수
  this.state = {message: 'test'} //this.state를 직접사용할 수 있음. setstate()사용하면 안됨
  console.log('constructor);
  this.handleClick = this.handleClick.bind(this);
}
```

- 컴포넌트가 마운트되기 전에 호출됨
- this.state에 객체를 할당하여 지역 state 초기화
- 인스턴스에 이벤트 처리 메서드 바인딩
- subscription 수행 등의 부수효과를 발생시키면 안됨.

#### static getDerivedStateFromProps()

```javascript
static getDerivedStateFromProps(props, state)
```

- 최초 마운트와 갱신시에 render 메서드 호출 직전에 호출됨.
- state 갱신을 위한 객체를 반환
- props에 state가 의존하는 경우에 <b>드물게</b> 사용
- 컴포넌트 인스턴스에 직접 접근할 수 없음
- 렌더링때마다 매번 실행됨

#### render()

```javascript
render() {
  (...)
  return (...)
}
```

- this.props와 this.state의 값을 활용하여 반환값 생성
  - React 엘리먼트
  - 배열, Fragment // TODO
  - Portal // TODO
  - 문자열, 숫자 : 텍스트 노드로 렌더링 됨
  - Boolean 또는 null : 아무것도 렌더링하지 않음. return test && <Child /> 패턴에 사용

#### componentDidMount()

- 컴포넌트가 마운트된 직후 호출
- 초기화작업, 네트워크요청, 데이터 구독 설정에 적절

#### shouldComponentUpdate(nextProps, nextState)

- props나 state가 새로운 값으로 갱신되어서 렌더링이 발생하기 직전에 호출됨. 기본값은 true
- 초기 렌더링, forceUpdate()때는 호출되지 않음
- 성능 최적화 용도. 렌더링 방지 목적으로 사용 시 버그 발생 가능성
- false 반환 시 render(), componentDidUpdate()가 호출되지 않음

#### getSnapshotBeforeUpdate(prevProps, prevState)

```javascript
class ScrollingList extends React.Component {
  constructor(props) {
    super(props);
    this.listRef = React.createRef();
  }

  getSnapshotBeforeUpdate(prevProps, prevState) {
    // Are we adding new items to the list?
    // Capture the scroll position so we can adjust scroll later.
    if (prevProps.list.length < this.props.list.length) {
      const list = this.listRef.current;
      return list.scrollHeight - list.scrollTop;
    }
    return null;
  }

  componentDidUpdate(prevProps, prevState, snapshot) {
    // If we have a snapshot value, we've just added new items.
    // Adjust scroll so these new items don't push the old ones out of view.
    // (snapshot here is the value returned from getSnapshotBeforeUpdate)
    if (snapshot !== null) {
      const list = this.listRef.current;
      list.scrollTop = list.scrollHeight - snapshot;
    }
  }

  render() {
    return <div ref={this.listRef}>{/* ...contents... */}</div>;
  }
}
```

- 가장 마지막으로 렌더링된 결과가 DOM에 반영되었을때 호출됨
- DOM으로부터 이전 상태의 스크롤위치등의 정보를 얻을 수 있음
- 반환값은 componentDidUpdate()에 전달 됨

#### componentWillUnmount()

- 컴포넌트가 마운트 해제되어 제거되기 직전에 호출됨.
- 타이머 제거, 네트워크 요청 취소, 구독 해제 등의 정리작업 수행
- 다시 렌더링되지 않으므로, setState()호출 하면 안됨