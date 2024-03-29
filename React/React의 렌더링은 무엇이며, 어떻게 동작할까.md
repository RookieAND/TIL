# 📖 Introduction

> React 에서 **렌더링** 이라는 단어는 귀가 빠지도록 들었다만.. 그게 정확히 무엇을 의미하는 걸까?

React는 사용자 인터페이스를 구축하기 위해 사용하는 프레임워크다. 하지만 나는 정작 렌더링이 정확히 무엇인지에 대해서는 알지 못했다.

# ✒️ Rendering in React

### ✏️ 리액트에서 렌더링이란 무엇인가?

- **렌더링 (Rendering)** 이란, 현재 컴포넌트의 props와 state의 상태에 기초하여 UI를 어떻게 구성할지 컴포넌트에게 요청하는 작업을 의미함.

### ✏️ 렌더링은 어떤 방식으로 진행되는가?

1. JSX 컴파일 후 React Element 수집

- 리액트는 컴포넌트 트리의 루트 노드부터 탐색하여 업데이트가 필요한 플래그가 지정되어있는 모든 컴포넌트를 찾는 과정을 상위에서 하위 노드로 반복한다.
- 클래스형 컴포넌트의 경우 `classComponentInstance.render()` 메서드를, 함수형 컴포넌트의 경우 `FunctionComponent(props)` 를 리액트에서 호출하고, 다음 과정을 위해 위해 반환 값을 저장한다.

```javascript
// JSX 문법으로 작성된 컴포넌트의 렌더링 결과:
return <MyComponent a={42} b="testing">Text here</MyComponent>

// Babel 에 의해 컴파일 되어 JS 파일로 변환된 결과
return React.createElement(MyComponent, {a: 42, b: "testing"}, "Text Here")

// React.createElement 메서드가 반환한 컴포넌트의 React Element
{type: MyComponent, props: {a: 42, b: "testing"}, children: ["Text Here"]}
```

- 컴포넌트의 렌더링 결과물은 JSX 문법으로 작성되었는데, 이는 babel에 의해 컴파일 되어 JS 파일로 변환된다.
- 이후 컴파일과 배포 준비가 모두 완료되었다면, `React.createElement()` 메서드를 호출하여 React Element 를 반환받는다.

2. 새로운 Virtual DOM Tree 생성 후 재조정

- 모든 컴포넌트 트리를 순회하여 얻은 렌더링 결과 값을 수집한 후, 리액트는 새롭게 생성된 가상 돔 트리와 기존의 트리를 비교하고 변경 사항을 수집한다.
- 이렇게 기존의 가상 돔 트리와 새롭게 생성된 가상 돔 트리를 상호 비교하는 과정을 **재조정 (reconciliation)** 이라고 한다.
- 이후 React 는 일련의 동기 시퀀스로 앞선 과정에서 계산된 결과를 실제 DOM에 일괄적으로 적용한다.

> React Element 란?

- React Element란 컴포넌트가 구성하고자 하는 UI의 정보를 담은 JS 객체이다.

### ✏️ 리액트의 렌더링과 커밋 단계

- React 팀은 상단의 작업을 렌더링과 커밋이라는 단계로 분류하였다.
  - **렌더링 단계 (Rendering Phase)** 는 컴포넌트를 렌더링하고 변경 사항을 계산하는 과정을 포함하였다. (비교 작업 수행)
  - **커밋 단계 (Commit Phase)** 는 계산된 변경 사항의 결과를 가지고 실제 DOM에 이를 적용하는 과정이다. (실질적인 변경)
- React 가 커밋 단계에서 DOM 에 변경 사항을 업데이트한 후, 모든 ref가 요청된 DOM 노드 및 컴포넌트 인스턴스를 가리키도록 모든 참조를 업데이트 한다.
- 이후 동기적으로 `componentDidMount` 나, `componentDidUpdate` 같은 클래스 LifeCycle 메소드를 실행시키거나, `useLayoutEffect` 훅을 실행한다.
- 이후 React는 잠시 짧은 timeout 을 세팅한 후, `useEffect` 을 실행시킨다. 이는 **Passive Effect** 단계라고도 부른다.
  - 따라서 `useEffect` 는 대부분 렌더링 이후에 동작하게 된다. DOM에 실제 요소가 반영된 후 약간의 텀을 두고 훅이 실행되도록 구조적으로 작성되었기 때문이다.

### ✏️ 렌더링은 DOM 업데이트를 의미하는 것이 아니다.

- 하지만 렌더링 과정이 DOM을 업데이트하는 과정과 **같지 않으며**, 시각적인 변경점이 없더라도 컴포넌트는 렌더링 될 수 있음을 유의해야 한다.
- 컴포넌트의 렌더링 결과가 이전과 같을 수 있으며, 동시성 모드에서는 다른 작업으로 인헤 렌더링이 무효화 되어 결과물을 버릴 수 있다

#### 📘 번외 : 왜 useEffect 는 Passive Effect 단계에서 별도로 실행될까?

- `useEffect` 는 비동기적으로 동작하기 때문에 작업에 대한 순서가 보장되지 않는다. 따라서 화면이 온전히 렌더링 된 후에 작업을 처리하도록 React 에서는 반드시 Passive Effect가 Paint 작업 이후에 실행되도록 하는 로직을 사용하였다. (추후 디테일하게 조사)
  - 만약 Commit Phase 에서 useEffect 와 Paint 작업이 동시에 동작하게 되었을 때, 리렌더링을 유발하는 요소가 useEffect 내부에 존재한다면 DOM 요소가 그려지기 전에 리렌더링이 발생한다.
- `useLayoutEffect` 의 경우 동기적으로 작동하기 때문에 작업에 대한 순서가 보장된다. 따라서 해당 훅에서 처리할 작업이 지연된다면 화면이 렌더링되지 않아 문제가 발생한다.

#### 📘 번외 : React 18에 추가된 동시성 개념.

1. Cocurrent Mode

- React 18에서는 `cocurrent mode` 를 활성화 하여 React 가 렌더링 단계에서 브라우저가 이벤트를 처리할 수 있도록 작업을 일시 중단할 수 있다.
- 이후 React는 해당 작업을 다시 시작하거나, 포기하거나, 재개할 수 있다. 렌더링 작업을 건너 뛰더라도 React는 커밋 단계를 동기적으로 실행한다.

2. Cocurrent Mode 란?

- Cocurrent Mode의 경우 렌더링 과정을 더 작은 Task로 나누고, 우선 순위를 매겨 중요도가 높은 작업부터 처리하고, 나머지는 잠시 중단시킨다.
- 이후 렌더링 결과를 DOM에 부분적으로 적용하고 이후 중요도가 낮은 작업을 마처 처리하여 최종적으로 렌더링 작업을 마친다.

3. 언제 쓸 수 있는가?

- 인터럽트 가능한 렌더링

  - Input 에 특정 단어를 타이핑하면 목록을 업데이트 하는 기능의 경우, 이벤트가 과도하게 호출되어 목록을 불러오는 요청이 잦아진다.
  - 기존의 Debounce, Throttle 로 과도한 함수 호출을 어느 정도 방지할 수는 있으나, 명확한 한계가 존재한다.
  - 이는 기존의 사용자 관점을 바탕으로 하여, 짧은 시간 안에 처리되어야 하는 작업을 우선적으로 처리할 수 있다는 이점을 가지게 된다.

- 로딩 시퀀스

  - 다른 페이지로 넘어가거나 서버로부터 정보를 받아올 때의 공백을 로딩바 혹은 스피너로 처리한다.
  - 하지만 이를 렌더링하는 작업이 페이지를 로드하는 작업에도 악영향을 줄 수 있다. (boolean setState => false to true 로 리렌더링)
  - Cocurrent Mode의 경우 이러한 로딩 state를 렌더링하는 과정이 새로운 화면을 준비하는 작업에 영향을 끼치지 않도록 한다.

4. useTransition Hook

   - React 18에서는 `useTransition` 훅을 통해 전환 업데이트를 명시적으로 구분하여 상태 업데이트를 진행할 수 있다.
   - 이전에는 모든 업데이트가 긴급하게 이루어졌기에, 상대적으로 느리게 업데이트 해도 괜찮은 작업도 즉각적인 렌더링이 이루어졌어야 했다.
   - 하지만 이제는 업데이트 작업 간의 **우선 순위** 를 두어서, 비교적 느긋하게 처리해도 되는 업데이트 작업의 경우 후순위로 미룰 수 있도록 설정이 가능하다.

# ✒️ How Does React handle Renders?

### ✏️ 렌더링 작업이 추가되는 경우

- 초기 렌더링 작업이 완료된 후, 리액트에게 리렌더링 작업을 추가시키는 작업들은 아래와 같다.
  - 함수형 컴포넌트의 경우 `useState` 의 setter 나, `useReducer` 의 dispatch 함수가 호출되었을 때.
  - 클래스형 컴포넌트의 경우 `this.setState()`, `this.forceUpdate()` 메서드가 호출되었을 때.
  - `useSyncExternalStore` Hook 이 호출되었을 때.

### ✏️ 일반적인 렌더링 동작

> React 에서는 부모 컴포넌트가 렌더링 되면, 자식 컴포넌트 또한 재귀적으로 렌더링 된다

- `A > B > C > D` 순으로 이루어진 컴포넌트 트리가 있고, `B` 컴포넌트가 렌더링 되었다고 가정해보자.
  - React 는 컴포넌트 트리의 루트 노드를 체크한다. `A` 컴포넌트는 렝더링 될 필요가 없다.
  - 이후 `B` 컴포넌트가 업데이트 되어야 함을 체크하고 렌더링을 진행한다. `B` 컴포넌트는 렌더링 결과로 `C` 를 반환한다.
  - `C` 컴포넌트는 본래 업데이트가 필요하지 않았으나 `B` 컴포넌트가 렌더링 되었으므로, React는 자식 컴포넌트인 `C` 를 리렌더링 하여 `D` 를 반환시킨다.
  - `D` 컴포넌트 또한 업데이트가 필요하지 않았으나 `C` 컴포넌트가 렌더링되었으므로 React는 `D` 컴포넌트 또한 렌더링 시킨다.
- React 는 렌더링 과정에서 props 의 변화에 대해서는 체크하지 않는다. 단지 부모 컴포넌트가 렌더링 되었는지만을 체크한다.
- 이는 컴포넌트 트리의 루트 노드가 `setState()` 호출이 어떠한 작업의 변화를 주지 않더라도 트리를 이루는 모든 요소들의 리렌더링을 유발시킴을 의미한다.
- 설령 리렌더링의 결과물이 이전과 동일하여 실제 DOM을 변경하지 않더라도, 두 가상 돔의 차이점을 비교하는 연산 (재조정) 은 시간과 노력이 필요하다.
- 리액트에서 렌더링은 실제 DOM 에 변경 사항을 업데이트 해야 하는지를 파악하는 작업이기도 하다.

### ✏️ React의 렌더링 규칙

- React 에서는 렌더링 작업이 반드시 기존의 props나 state를 변경하게끔 하지 않아야 하고, 어떠한 부작용도 없어야 하는 규칙이 존재한다.
- 일례로 `console.log()` 의 경우 새로이 추가되었을 때 화면에 로그가 찍히는 효과가 추가로 발생하지만, 어떠한 상태도 건드리지 않는다.
- 하지만 렌더링 과정에서 props 나 state를 의도적으로 수정한다면, 이전과 같은 작업을 수행할 수 없으며 무결점성 또한 크게 해치게 된다.

1. 렌더링 로직 과정에서 진행할 수 없는 작업
   - 컴포넌트 내에 존재하는 변수 혹은 Object를 수정하는 행위
   - `Math.random()`, `Date.now()` 같은 무작위한 값을 생성하는 행위
   - 네트워크 요청을 하는 행위
   - `setState()` 를 호출하는 행위
