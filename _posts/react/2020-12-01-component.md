---
title: "React Component"
layout: post
categories:
  - react
tags:
  - concept
  - component
last modified_at: 2020-12-01T22:32:10
---

클래스형 vs 함수형

1. 클래스형 컴포넌트

    ```javascript
    class ClassComponent extends Component {
      const {message, name} = this.props;
      handleChange = e => {
        this.setState({
          message: e.target.value;
        });
      }
      render() {
        return (
          <div>
            Class Component
            <input onChange={this.handleChange}/>
          </div>
        );
      }
    }
    export default ClassComponent;
    ```

    1. state
      * constructor에서 this.state 초기값 설정
      * constructor 없이 초기값 설정
      * 객체 형식
    2. props
      * 컴포넌트의 속성을 설정. 읽기 전용
      * this.props
    3. LifeCycle 함수 사용
    4. 이벤트 핸들링
      * 클래스 내부에 생성
    5. 생명주기 메서드에 다수의 로직을 구현해야 하는 경우가 많아, 코드 재사용성이 좋지 않음  
<br/>

2. 함수형 컴포넌트  

    ```javascript
    function FuncComponent() {
      const name = 'react';
      return <div>{name}</div>;
    }
    export default FuncComponent;
    ```


    1. 16.8에 Hook이 추가되면서 함수형 컴포넌트 활용성이 높아짐
    2. Hook을 사용해서 props, state, LifeCycle을 처리할 수 있게 됨
    3. 코드 재사용성을 높일 수 있음