# π Introduction

> μλ¬΄λ¦¬ setStateλ₯Ό λ§μ΄ λ£μ΄λ λ¦¬λ λλ§μ΄ νλ²λ§ μΌμ΄λλ μ΄μ λ λ¬΄μμΈκ°?

Reactλ₯Ό μ¬μ©νλ€ λ³΄λ©΄ νμ°μ μΌλ‘ stateλ₯Ό λ€λ£° μλ°μ μκ² λκ³ , μ΄λ₯Ό μ¬μ©νλ€ λ³΄λ©΄ λ¬Έλ λλ ν κ°μ§ μλ¬Έμ΄ μ‘΄μ¬νλ€. λΆλͺ setState ν¨μλ λ¦¬λ λλ§μ μ λ°νλ€κ³  νλλ° μ μ¬λ¬λ² μ€νμ ν΄λ ν λ²λ§ λ¦¬λ λλ§μ΄ μ§νλλ κ±ΈκΉ? μ§κΈμ React μμ μ΄λ₯Ό Batching μ²λ¦¬νμ¬ μΌκ΄μ μΌλ‘ μ²λ¦¬ν¨μ μκ³  μμμ§λ§, React 18μμ μκ°νλ Automatic Batchingμ κΈ°μ‘΄μ Batching μμμ κ°μ νλ€κ³  λ§νκΈΈλ μ΄λ€ λΆλΆμ κ°μ νλμ§κ° κ΅μ₯ν κΆκΈνμλ€. λ°λΌμ κ³΅μ λ¬Έμμ Reactμ λ©μΈνμ΄λ λΆκ»μ μμ±ν κΈμ ν λλ‘ React μμλ state update μμμ μ΄λ»κ² μ²λ¦¬νλμ§ νν€μ³λ³΄κ³ μ νλ€.

# βοΈ Auto Batching

### βοΈ Batching μ΄λ?

- `state` κ°μ΄ λ³κ²½λμμ κ²½μ° React μμλ ν΄λΉ μ»΄ν¬λνΈλ₯Ό λ¦¬λ λλ§ νλ©°, λΆνμν λ¦¬λ λλ§μ λ°©μ§νκΈ° μν΄ stateλ₯Ό λ³κ²½νλ μμμ **μΌκ΄μ μΌλ‘ μ²λ¦¬**νλ€.
- μ΄λ κ² `state` μ μλ°μ΄νΈ μμμ λͺ¨μ μΌκ΄ μ²λ¦¬νλ λ°©μμ **Batching** μ΄λΌκ³  νλ©°, μ΄ λμ React μμλ λΆνμν λ¦¬λ λλ§μ λ°©μ§ν  μ μκ² λμλ€.

```jsx
import { useState } from "react";

function Counter() {
	const [count, setCount] = useState(0);

	function increaseCountThree() {
		// μλμ μμμ λͺ¨λ μΌκ΄μ μΌλ‘ λ¬Άμ¬ μ²λ¦¬λλ€. ν λ²μ λ¦¬λ λλ§λ§ λ°μνλ€.
		setCount((prev) => prev + 1);
		setCount((prev) => prev + 1);
		setCount((prev) => prev + 1);
	}

	return (
		<div>
			<button onClick={increaseCountThree}>+1</button>
			<p>Count : {count}</p>
		</div>
	);
}

export default Counter;
```

- μλ¨μ μ½λλ setter ν¨μλ₯Ό **μΈ μ°¨λ‘ μ€ν**μμΌ°κΈ° λλ¬Έμ λ¦¬λ λλ§λ μΈ λ² λ°μν  κ² κ°μ§λ§, μ€μ λ‘ μ½λλ₯Ό μ€νν΄λ³΄λ©΄ **λ¦¬λ λλ§μ νλ²λ§ λ°μνλ€**.
- React λ μ¬λ¬ λ²μ state update μμμ Queueμ λͺ°μλ£κ³  μΌμ  μ£ΌκΈ°λ§λ€ Queueμ λ±λ‘λ μμμ μμ°¨μ μΌλ‘ μΌκ΄ μννλ©΄μ λΆνμν λ¦¬λ λλ§μ λ°©μ§νλ€.

### βοΈ React 18μμ μΆκ°λ Automatic Batching μ΄λ?

- React 18 λ²μ  μ΄νμμλ μ€μ§ React μ **μ΄λ²€νΈ νΈλ€λ¬ λ΄λΆμ** state update μμμ λν΄μλ§ Batching μ΄ κ°λ₯νλ€. νμ§λ§ Promiseλ setTimeout λ΄λΆμ μμμ λΆκ°λ₯νλ€.
- μλνλ©΄ μ΄μ μλ **λΈλΌμ°μ μ μ΄λ²€νΈκ° μ€νλλ μ€μλ§** Batching μμμ μννκΈ° λλ¬Έμ΄λ€. λ°λΌμ μ΄λ²€νΈκ° μ’λ£λ νμ μ€νλλ κ²½μ°λ Batching μμμ΄ λΆκ°λ₯νλ€.
- νλ¨μ μ½λμ κ²½μ° state update μμμ΄ λΉλκΈ°μ μΌλ‘ μ²λ¦¬λμ΄ Eventκ° μ’λ£λ νμ μ€νλκΈ° λλ¬Έμ, Reactμ Batching μμμ κ±Έλ¦¬μ§ μμ λ μ°¨λ‘ λ¦¬λ λλ§μ μ λ°μμΌ°λ€.

```jsx
function App() {
	const [count, setCount] = useState(0);
	const [flag, setFlag] = useState(false);

	function handleClick() {
		fetchSomething().then(() => {
			// React 17 μ΄μ μ λ²μ μμλ ν΄λΉ μμμ Batching μ²λ¦¬νμ§ μλλ€.
			// μλνλ©΄ ν΄λΉ μμμ μ΄λ²€νΈκ° μ’λ£λ μ΄ν (100ms λ€) μ μ€νλκΈ° λλ¬Έμ΄λ€.
			setCount((c) => c + 1); // λ¦¬λ λλ§ μ λ°
			setFlag((f) => !f); // λ¦¬λ λλ§ μ λ°
		});
	}

	return (
		<div>
			<button onClick={handleClick}>Next</button>
			<h1 style={{ color: flag ? "blue" : "black" }}>{count}</h1>
		</div>
	);
}

function fetchSomething() {
	return new Promise((resolve) => setTimeout(resolve, 100));
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

- νμ§λ§ React 18 λ²μ  μ΄νλΆν°λ λ¨μν μ΄λ²€νΈ νΈλ€λ¬ λ΄λΆ λΏλ§μ΄ μλλΌ Promiseλ setTimeout, Native Event Handler κ°μ μμμ λν΄μλ Batching μμμ **μλμΌλ‘** μννκ² ν΄μ£Όμλ€.
- React 18 μμ μ κ³΅νλ `ReactDOM.createRoot` λ©μλλ₯Ό κΈ°λ°μΌλ‘ λ λλ§μ μ§νν  κ²½μ° λͺ¨λ  state update μμμ μλμΌλ‘ Batching μ²λ¦¬λλ€. μ΄ κΈ°λ₯μ **Automatic Batching ** μ΄λΌκ³  νλ€.

```jsx
function App() {
	const [count, setCount] = useState(0);
	const [flag, setFlag] = useState(false);

	function handleClick() {
		fetchSomething().then(() => {
			// React 18 μ΄νμμλ ν΄λΉ μμμ Batching μ²λ¦¬ νλ€.
			setCount((c) => c + 1);
			setFlag((f) => !f);
			// React λ ν΄λΉ μμμ μΌκ΄ μ²λ¦¬νμ¬ ν λ²μ λ¦¬λ λλ§λ§ μ§ννλ€.
		});
	}

	return (
		<div>
			<button onClick={handleClick}>Next</button>
			<h1 style={{ color: flag ? "blue" : "black" }}>{count}</h1>
		</div>
	);
}

function fetchSomething() {
	return new Promise((resolve) => setTimeout(resolve, 100));
}

const rootElement = document.getElementById("root");
// This opts into the new behavior!
ReactDOM.createRoot(rootElement).render(<App />);
```

### βοΈ ReactDOM.flushSync() λ?

- react-dom λΌμ΄λΈλ¬λ¦¬μ μΆκ°λ `ReactDOM.flushSync()` λ©μλλ **Auto Batching μ λ¬΄μνκ³ ** μ¦μ DOMμ λ λλ§ν΄μ€λ€.
- React μμλ κ³΅μμ μΌλ‘ ν΄λΉ λ©μλμ μ¬μ©μ μΆμ²νμ§ μμΌλ©° (de-opt case), νμν μν©μ΄ μμ κ²½μ°μλ§ μ¬μ©ν  κ²μ κ°μ‘°νλ€.

```jsx
import { flushSync } from "react-dom";

function handleClick() {
	// React λ flushSync λ©μλκ° μ€νλλ μ¦μ DOMμ μλ°μ΄νΈ νλ€.
	flushSync(() => {
		setCounter((c) => c + 1);
	});
	// React λ flushSync λ©μλκ° μ€νλλ μ¦μ DOMμ μλ°μ΄νΈ νλ€.
	flushSync(() => {
		setFlag((f) => !f);
	});
	// λ°λΌμ ν΄λΉ ν¨μκ° μ€νλ  κ²½μ° Reactλ μ΄ λ λ²μ λ¦¬λ λλ§μ μννλ€.
}
```

# π References

- https://beta.reactjs.org/blog/2022/03/08/react-18-upgrade-guide#automatic-batching
- https://github.com/reactwg/react-18/discussions/21
