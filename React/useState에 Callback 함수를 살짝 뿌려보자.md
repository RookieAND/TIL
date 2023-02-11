# 📖 Introduction

# ✒️ state in React

### ✏️ What is State?

- 컴포넌트는 사용자와의 상호작용 결과로서 화면을 변경해줘야 할 때가 종종 있다. 예를 들면 사용자가 Form에 정보를 입력했다던가, 버튼을 클릭하여 캐러셀에서 다음 이미지를 보여주게 한다던가..
- 이렇게 컴포넌트는 **특정한 값들을 저장해야 할 필요**가 있으며, React 에서는 컴포넌트에서 사용하는 이러한 종류의 메모리들을 `state` 라고 정의내렸다.
- `state` 값이 변경되었을 경우 React 는 해당 컴포넌트를 리렌더링 한다. 따라서 `state` 의 변화는 컴포넌트의 리렌더링을 유발시킨다고 볼 수 있다.

- `state` 값이 변경되었을 경우 React 에서는 해당 컴포넌트를 리렌더링 하며, 불필요한 리렌더링을 방지하기 위해 **16ms 주기**로 state를 변경하는 작업을 **일괄적으로 처리**한다.
- 이렇게 `state` 의 업데이트 작업을 한번에 모아 순차적으로 처리하는 방식을 **auto batching** 이라고 하며, 이 덕분에 React 에서는 state를 비동기적으로 처리할 수 있게 된다.

### ✏️ local Variable doesn't trigger re-render

- 하단의 코드는 사용자가 `+1`, `-1` 버튼을 눌렀을 경우 이에 맞춰서 count 변수의 값을 변경시키는 이벤트 핸들러를 버튼에 할당시켰다.
- 하지만 해당 코드를 실행시키고 버튼을 누르면 우리가 기대했던 결과가 나오지 않게 된다. 버튼을 눌러도 Count는 계속 `0` 이 찍힌다.

```jsx
function App() {
	let count = 0;

	function increaseCount() {
		count += 1;
		console.log(count);
	}

	function decreaseCount() {
		count -= 1;
		console.log(count);
	}

	return (
		<div>
			<button onClick={increaseCount}>+1</button>
			<button onClick={decreaseCount}>-1</button>
			<p>Count : {count}</p>
		</div>
	);
}

export default App;
```

- 지역 변수 `count` 가 컴포넌트가 새롭게 리렌더링 될 때마다 `0` 으로 초기화 되기 때문이다. React는 지역 변수의 값이 변경되어도 이를 리렌더링에 반영하지 않는다.
- 사실 더 근본적인 이유는 우리가 컴포넌트 내부에 정의한 지역 변수의 값이 변경되었다 하더라도 React에서는 컴포넌트를 **리렌더링 하지 않는다** 는 것이다.
- 실제로 `console.log` 에는 count의 변경 값이 잘 찍히지만 **리렌더링이 되지 않았기 때문에** 화면에는 컴포넌트가 초기에 렌더링 되었을 때의 `count` 값인 0을 그대로 유지한다.
- 따라서 React 에서는 값이 변경되었을 때 리렌더링을 진행하도록 하면서, 컴포넌트가 새롭게 렌더링이 되었더라도 변경된 값을 그대로 유지하도록 하는 장치를 `state` 로 구현하였다.

### ✏️ useState Hook

- `useState` 훅은 컴포넌트가 기억하고자 하는 값, 즉 `state` 를 컴포넌트에 추가할 수 있도록 해주는 React Hook 이다.
- 인자로는 state의 초기 값 (initialValue) 을 넘겨줄 수 있으며 원시 값, 객체 뿐만 아니라 콜백 함수도 초기 값으로 넣어줄 수 있다.
- `useState` 은 state와 setter 함수를 하나의 배열로 반환하여 넘겨주며, 일반적으로 비구조화 할당을 통해 두 개의 값을 인계 받는다.

```jsx
import { useState } from "react";

function App() {
	const [count, setCount] = useState(0); // 초기 값 : 0

	function increaseCount() {
		setCount(count + 1);
		console.log(count);
	}

	function decreaseCount() {
		setCount(count + 1);
		console.log(count);
	}

	return (
		<div>
			<button onClick={increaseCount}>+1</button>
			<button onClick={decreaseCount}>-1</button>
			<p>Count : {count}</p>
		</div>
	);
}

export default App;
```

- 지역 변수로 구현되었던 코드를 `useState` 훅으로 재구성한 코드이다. 이제 정상적으로 카운터가 작동된다!

### ✏️ state is private, and isolated

-

# ✒️ Callback in initialValue

# ✒️ Callback in setState

### ✏️ SubTitle

# 📒 References
