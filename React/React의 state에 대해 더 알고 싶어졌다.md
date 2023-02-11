# 📖 Introduction

> 아무리 setState를 많이 넣어도 리렌더링이 한번만 일어나는 이유는 무엇인가?

# ✒️ state in React

### ✏️ What is State?

- 컴포넌트는 사용자와의 상호작용 결과로서 화면을 변경해줘야 할 때가 종종 있다. 예를 들면 사용자가 Form에 정보를 입력했다던가, 버튼을 클릭하여 캐러셀에서 다음 이미지를 보여주게 한다던가..
- 이렇게 컴포넌트는 **특정한 값들을 저장해야 할 필요**가 있으며, React 에서는 컴포넌트에서 사용하는 이러한 종류의 메모리들을 `state` 라고 정의내렸다.
- `state` 값이 변경되었을 경우 React 는 해당 컴포넌트를 리렌더링 한다. 따라서 `state` 의 변화는 컴포넌트의 리렌더링을 유발시킨다고 볼 수 있다.

### ✏️ local Variable doesn't trigger re-render

- 하단의 코드는 사용자가 `+1`, `-1` 버튼을 눌렀을 경우 이에 맞춰서 count 변수의 값을 변경시키는 이벤트 핸들러를 버튼에 할당시켰다.
- 하지만 해당 코드를 실행시키고 버튼을 누르면 우리가 기대했던 결과가 나오지 않게 된다. 버튼을 눌러도 Count는 계속 `0` 이 찍힌다.

```jsx
function Counter() {
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

export default Counter;
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

function Counter() {
	const [count, setCount] = useState(0);

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

export default Counter;
```

- 지역 변수로 구현되었던 코드를 `useState` 훅으로 재구성한 코드이다. 이제 정상적으로 카운터가 작동된다!

### ✏️ State is private, and isolated

- state 는 자신이 속한 컴포넌트에 종속적이다. 만약 같은 컴포넌트를 두 차례 렌더링 하더라도, 각 컴포넌트들에 속한 state는 서로 분리되어 작동하기 된다.

```jsx
import Counter from "./Counter";

function App() {
	return (
		<div>
			<Counter />
			<Counter />
		</div>
	);
}

export default App;
```

- 사전에 제작한 Counter 컴포넌트를 두 차례 렌더링하고, 두 컴포넌트 내에 있는 버튼을 누르게 되면, state가 서로 **간섭하지 않고 독립적으로 작동함**을 알 수 있다.
- 이로서 state는 자신이 속한 컴포넌트에 지역적으로 관리되며, Counter 컴포넌트를 렌더링하는 상위 컴포넌트인 App 에도 Counter 내의 state가 어떻게 작동하는지 알 수 없다.
- 만약 부모 컴포넌트의 state 가 변경되었을 때 자식 컴포넌트에도 이를 적용시키기 위해서는 **props drilling**, **Context API** 와 같은 방법들을 택해야 한다.

# ✒️ Update state

### ✏️ React batches state updates

- `state` 값이 변경되었을 경우 React 에서는 해당 컴포넌트를 리렌더링 하며, 불필요한 리렌더링을 방지하기 위해 state를 변경하는 작업을 **일괄적으로 처리**한다.
- 이렇게 `state` 의 업데이트 작업을 모아 일괄 처리하는 방식을 **Batching** 이라고 하며, 이 덕에 React 에서는 불필요한 리렌더링을 방지할 수 있게 되었다.
- 또한 Batching 작업을 통해 컴포넌트 내부의 일부 state update 작업이 실행되지 않은 상태로 리렌더링이 진행되는 **half-finished** 상태를 막을 수도 있다.

```jsx
function changeState() {
	const [flag, setFlag] = useState(false);
	// setFlag (setter) 함수가 세 차례 실행되었지만, 리렌더링은 단 한번만 발생하게 된다.
	setFlag((prevFlag) => !prevFlag); // false => true
	setFlag((prevFlag) => !prevFlag); // false => true
	setFlag((prevFlag) => !prevFlag); // false => true
}
```

### ✏️ State is a snapshot

- React 에서 컴포넌트를 렌더링한다는 의미는 컴포넌트 함수를 호출한다는 의미와 일맥상통한다. 따라서 함수를 호출하여 반환된 JSX 는 컴포넌트가 렌더링된 순간의 **스냅샷** 임을 의미한다.
- state는 컴포넌트 함수 바깥에서 Closure 형태로 관리되는 변수이다. React 는 컴포넌트 함수가 호출된 순간의 state 값 (스냅샷) 을 제공하며 이는 다음 렌더링 이전까지 변하지 않는다.

```jsx
import { useState } from "react";

function Counter() {
	// 초기 렌더링 시 count 값은 0으로 고정된다. 이는 다음 렌더링 이전까지 절대 변경되지 않는다.
	const [count, setCount] = useState(0);

	function increaseCount() {
		// 초기 렌더링 이후 count 값은 0으로 고정된다.
		setCount(count + 1); // 0 + 1 = 1
		setCount(count + 1); // 0 + 1 = 1, 이전의 결과에 영향을 받지 않고 현재 count 값을 적용한다.
		setCount(count + 1); // 0 + 1 = 1, 이전의 결과에 영향을 받지 않고 현재 count 값을 적용한다.
	}

	return (
		<div>
			<button onClick={increaseCount}>+3</button>
			<p>Count : {count}</p>
		</div>
	);
}

export default Counter;
```

- 상단의 코드는 총 세 번의 setState 작업을 수행했지만 리렌더링 후 count 값은 기대와 달리 `0` 에서 `1` 로 변경된다.
- 현재 `count` state 값은 초기 렌더링을 진행한 후 0으로 고정되었다. 따라서 `setCount(count + 1)` 작업의 결과는 0에 1을 더한 값으로 state를 업데이트 할 것이다.
- 상단의 코드에서는 이 작업을 세 차례 수행하였으나, `count` 값은 현재 0으로 고정된 상태기에 결국 여러번 작업을 수행해도 리렌더링 이후 결과는 1로 변경될 것이다.

```jsx
import { useState } from "react";

function Counter() {
	// 초기 렌더링 시 count 값은 0으로 고정된다. 이는 다음 렌더링 이전까지 절대 변경되지 않는다.
	const [count, setCount] = useState(0);

	function increaseCount() {
		// 리렌더링 이후 count 값은 3으로 변경된다. 현재 count 값은 0임에 유의
		setCount(count + 100); // 0 + 100 = 100
		setCount(count - 200); // 0 - 200 = -200
		setCount(count + 1); // 0 + 1 = 1, 최종적으로 count 값은 1로 수정되어 다음 렌더링에 반영됨
	}

	return (
		<div>
			<button onClick={increaseCount}>+1</button>
			<p>Count : {count}</p>
		</div>
	);
}

export default Counter;
```

- 상단의 코드는 총 세 번의 setState 작업을 수행하였고, 리렌더링 이후 count 값은 `1` 로 변경되었다.
- 여기서도 초기 렌더링 시 count 값이 `0` 임을 유의해야 한다. 세 번의 `setCount` 함수가 호출되었으나 인자로 받은 count 값은 `0` 으로 고정된다.
- 따라서 **가장 마지막에 실행된** `setCount` 의 결과가 최종적으로 변경된 count 값이 되고, 리렌더링 이후 count 값은 `1` 으로 수정되는 것이다.

```jsx
import { useState } from "react";

function Counter() {
	const [number, setNumber] = useState(0);

	return (
		<div>
			<h1>{number}</h1>
			<button
				onClick={() => {
					setNumber(number + 5);
					setTimeout(() => {
						alert(number);
					}, 3000);
				}}
			>
				+5
			</button>
		</div>
	);
}
export default Counter;
```

- 상단의 코드는 버튼을 클릭할 경우 `number` state 에 5를 더하고, setTimeout 함수를 사용하여 3초 후에 `number` state를 alert 창에 띄우도록 하였다.
- 하지만 3초 후 alert 창에는 5가 아닌 `0` 이 띄워진다. 왜냐하면 alert 함수에 인자로 들어간 number는 사용자가 **버튼을 클릭했을 때의 state 값**을 담았기 때문이다.
- 설령 alert 함수가 리렌더링 이후에 진행되더라도, 해당 작업이 스케줄러에 등록될 당시의 state 값은 **리렌더링이 일어나기 전의 값**이기에 이런 현상이 발생하는 것이다.
- 결국 state 는 다음 렌더링이 발생하지 않는 이상 **현재의 값을 유지한다.** (불변하다) 설령 이벤트 핸들러 내부의 코드가 비동기적으로 작동하더라도 말이다.

### ✏️ functional Update

# ✒️ Usage of useState Hook

### ✏️ Callback in initialValue

# 📒 References

- https://beta.reactjs.org/learn/state-a-components-memory
- https://beta.reactjs.org/learn/state-as-a-snapshot#rendering-takes-a-snapshot-in-time
- https://github.com/reactwg/react-18/discussions/5
