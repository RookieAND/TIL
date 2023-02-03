# ✒️ useEffect VS useLayoutEffect

### ✏️ useEffect

- `useEffect` 는 일반적으로 React의 Commit Phase를 마친 후, 브라우저가 **렌더링 된 후**에 실행된다.
- 따라서 `useEffect` 훅은 대부분의 경우 브라우저의 Paint 작업을 마친 이후 실행된다.
- React는 렌더링 단계에서 어떠한 Side Effect도 허용하지 않기 때문에, 렌더링 이후에 작업을 수행하는 useEffect에 Side Effect를 시행하는 것을 추천한다.

### ✏️ useLayoutEffect

- `useLayoutEffect` 훅은 컴포넌트가 commit Phase에서 DOM이 수정된 이후에 동기적으로 실행된다.
- 이는 React에서 Paint 작업을 의도적으로 늦추는 결과를 야기하며, useLayoutEffect 에서 실행된 작업은 반드시 Paint 과정 이전에 종료된다.

### ✏️ Difference

> React 에서 Side Effect 란?

- React 컴포넌트가 브라우저에 렌더링 된 이후에 비동기적으로 처리되어야 하는 부수적인 효과들을 의미한다.
- 즉, React 의 컴포넌트가 컨트롤 할 수 없는 위치에서 작동되는 모든 작업이 Side Effect라고 필자는 이해했다.

- 외부 API를 호출하여 비동기적으로 데이터를 가져오는 작업
- setTimeout(), setInterval() 등과 같은 타이머 함수를 작동
- 사용자가 직접 컴포넌트 내부의 DOM 요소를 수정하는 작업
- 함수형 컴포넌트가 매개변수로 받은 값들을 수정하는 작업

- 이러한 Side Effect 를 포함하지 않은 컴포넌트를 Pure Component 라고 지칭한다.
- React에서는 Pure Component가 동일한 props와 state 값을 가진다면 이전과 같은 결과가 렌더링 된다고 하였다.
