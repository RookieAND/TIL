# π Introduction

> React μμ state λ₯Ό μΈ μ€ λͺ¨λ₯Έλ€λ κ±΄ Reactλ₯Ό λ€λ£° μ€ λͺ¨λ₯Έλ€λ λ»μ΄ μλκΉ.

state, Reactλ₯Ό μ²μ μλ¬Ένμ λΉμ λμκ²λ λλ¬΄λ μμν κ°λμ΄μλ€. κ΅³μ΄ μ§μ­ ν¨μκ° μλ λ³λμ state λΌλ κ°λμ μ±μ©ν μ΄μ κ° λ¬΄μμΈκ°? μ΄λ° λμ μλ¬Έμ Reactμ μνκ³λ₯Ό κ³΅λΆνλ©΄μ νλμ© νλ Έλ€.
νμ§λ§ μμ§λ λλ stateλ₯Ό μλ²½ν μ΄ν΄νλ€κ³  λ§νκΈ° μ΄λ ΅λ€. ν¨μν μλ°μ΄νΈλ₯Ό μ§ννλ©΄ λκΈ°μ μΌλ‘ μλνλ κ² μλκ°? λΌλ μλͺ»λ μκ°μ μ΅κ·ΌμμΌ λ°λ‘μ‘κ² λ μ΄ν μ΄λ¬ν μκ°μ λμ± μ¬νλμλ€.
λ°λΌμ λ μ μ‘μμ stateμ κ΄λ ¨λ κ°λμ μ΅λν κΉ¨λΆμκ³ , λνμΌν μ€λͺμ κ²½μ°μλ μΆκ°μ μΌλ‘ React μ½λλ₯Ό μ’ λ―μ΄λ³΄λ©΄μ μλ μλ¦¬λ₯Ό μ΄ν΄νκ³ μ νλ€. νμ§λ§ React μμ€ μ½λμ λ³΅μ‘λλ.. μ΄ν μλ΅..

# βοΈ state in React

### βοΈ What is State?

- μ»΄ν¬λνΈλ μ¬μ©μμμ μνΈμμ© κ²°κ³Όλ‘μ νλ©΄μ λ³κ²½ν΄μ€μΌ ν  λκ° μ’μ’ μλ€. μλ₯Ό λ€λ©΄ μ¬μ©μκ° Formμ μ λ³΄λ₯Ό μλ ₯νλ€λκ°, λ²νΌμ ν΄λ¦­νμ¬ μΊλ¬μμμ λ€μ μ΄λ―Έμ§λ₯Ό λ³΄μ¬μ£Όκ² νλ€λκ°..
- μ΄λ κ² μ»΄ν¬λνΈλ λ‘μ§μ μ²λ¦¬λ₯Ό μν΄ **νΉμ ν κ°λ€μ μ μ₯ν΄μΌ ν  νμ** κ° μμΌλ©°, React μμλ μ΄λ₯Ό `state` λΌκ³  μ μνμλ€.
- `state` κ°μ΄ λ³κ²½λμμ κ²½μ° React λ ν΄λΉ μ»΄ν¬λνΈλ₯Ό λ¦¬λ λλ§ νλ€. λ°λΌμ `state` μ λ³νλ μ»΄ν¬λνΈμ λ¦¬λ λλ§μ μ λ°μν¨λ€κ³  λ³Ό μ μλ€.

### βοΈ local Variable doesn't trigger re-render

- νλ¨μ μ½λλ μ¬μ©μκ° `+1`, `-1` λ²νΌμ λλ μ κ²½μ° μ΄μ λ§μΆ°μ count λ³μμ κ°μ λ³κ²½μν€λ μ΄λ²€νΈ νΈλ€λ¬λ₯Ό λ²νΌμ ν λΉμμΌ°λ€.
- νμ§λ§ ν΄λΉ μ½λλ₯Ό μ€νμν€κ³  λ²νΌμ λλ₯΄λ©΄ μ°λ¦¬κ° κΈ°λνλ κ²°κ³Όκ° λμ€μ§ μκ² λλ€. λ²νΌμ λλ¬λ Countλ κ³μ `0` μ΄ μ°νλ€.

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

- μ§μ­ λ³μ `count` κ° μ»΄ν¬λνΈκ° μλ‘­κ² λ¦¬λ λλ§ λ  λλ§λ€ `0` μΌλ‘ μ΄κΈ°ν λκΈ° λλ¬Έμ΄λ€. Reactλ μ§μ­ λ³μμ κ°μ΄ λ³κ²½λμ΄λ μ΄λ₯Ό λ¦¬λ λλ§μ λ°μνμ§ μλλ€.
- μ¬μ€ λ κ·Όλ³Έμ μΈ μ΄μ λ μ°λ¦¬κ° μ»΄ν¬λνΈ λ΄λΆμ μ μν μ§μ­ λ³μμ κ°μ΄ λ³κ²½λμλ€ νλλΌλ Reactμμλ μ»΄ν¬λνΈλ₯Ό **λ¦¬λ λλ§ νμ§ μλλ€** λ κ²μ΄λ€.
- μ€μ λ‘ `console.log` μλ countμ λ³κ²½ κ°μ΄ μ μ°νμ§λ§ **λ¦¬λ λλ§μ΄ λμ§ μμκΈ° λλ¬Έμ** νλ©΄μλ μ»΄ν¬λνΈκ° μ΄κΈ°μ λ λλ§ λμμ λμ `count` κ°μΈ 0μ κ·Έλλ‘ μ μ§νλ€.
- λ°λΌμ React μμλ κ°μ΄ λ³κ²½λμμ λ λ¦¬λ λλ§μ μ§ννλλ‘ νλ©΄μ, μ»΄ν¬λνΈκ° μλ‘­κ² λ λλ§μ΄ λμλλΌλ λ³κ²½λ κ°μ κ·Έλλ‘ μ μ§νλλ‘ νλ μ₯μΉλ₯Ό `state` λ‘ κ΅¬ννμλ€.

### βοΈ useState Hook

- `useState` νμ μ»΄ν¬λνΈκ° κΈ°μ΅νκ³ μ νλ κ°, μ¦ `state` λ₯Ό μ»΄ν¬λνΈμ μΆκ°ν  μ μλλ‘ ν΄μ£Όλ React Hook μ΄λ€.
- μΈμλ‘λ stateμ μ΄κΈ° κ° (initialValue) μ λκ²¨μ€ μ μμΌλ©° μμ κ°, κ°μ²΄ λΏλ§ μλλΌ μ½λ°± ν¨μλ μ΄κΈ° κ°μΌλ‘ λ£μ΄μ€ μ μλ€.
- `useState` μ stateμ setter ν¨μλ₯Ό νλμ λ°°μ΄λ‘ λ°ννμ¬ λκ²¨μ£Όλ©°, μΌλ°μ μΌλ‘ λΉκ΅¬μ‘°ν ν λΉμ ν΅ν΄ λ κ°μ κ°μ μΈκ³ λ°λλ€.

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

- μ§μ­ λ³μλ‘ κ΅¬νλμλ μ½λλ₯Ό `useState` νμΌλ‘ μ¬κ΅¬μ±ν μ½λμ΄λ€. μ΄μ  μ μμ μΌλ‘ μΉ΄μ΄ν°κ° μλλλ€!

### βοΈ State is private, and isolated

- state λ μμ μ΄ μν μ»΄ν¬λνΈμ μ’μμ μ΄λ€. λ§μ½ κ°μ μ»΄ν¬λνΈλ₯Ό λ μ°¨λ‘ λ λλ§ νλλΌλ, κ° μ»΄ν¬λνΈμ μν stateλ μλ‘ λλ¦½μ μΌλ‘ μλνλ€.

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

- μ¬μ μ μ μν Counter μ»΄ν¬λνΈλ₯Ό λ μ°¨λ‘ λ λλ§νκ³ , λ μ»΄ν¬λνΈ λ΄μ μλ λ²νΌμ λλ₯΄κ² λλ©΄, stateκ° μλ‘ **κ°μ­νμ§ μκ³  λλ¦½μ μΌλ‘ μλν¨**μ μ μ μλ€.
- μ΄λ‘μ stateλ μμ μ΄ μν μ»΄ν¬λνΈμ λλ¦½μ μΌλ‘ κ΄λ¦¬λλ©°, μ΄λ‘ μΈν΄ Counter μ»΄ν¬λνΈλ₯Ό λ λλ§νλ μμ μ»΄ν¬λνΈμΈ App μλ Counter λ΄μ stateκ° μ΄λ»κ² μλνλμ§ μ μ μλ€.
- λ§μ½ λΆλͺ¨ μ»΄ν¬λνΈμ state κ° λ³κ²½λμμ λ μμ μ»΄ν¬λνΈμλ μ΄λ₯Ό μ μ©μν€κΈ° μν΄μλ **props drilling**, **Context API** μ κ°μ λ°©λ²λ€μ νν΄μΌ νλ€.

# βοΈ Update state

### βοΈ React batches state updates

- `state` κ°μ΄ λ³κ²½λμμ κ²½μ° React μμλ ν΄λΉ μ»΄ν¬λνΈλ₯Ό λ¦¬λ λλ§ νλ©°, λΆνμν λ¦¬λ λλ§μ λ°©μ§νκΈ° μν΄ stateλ₯Ό λ³κ²½νλ μμμ **μΌκ΄μ μΌλ‘ μ²λ¦¬**νλ€.
- μ΄λ κ² `state` μ μλ°μ΄νΈ μμμ λͺ¨μ μΌκ΄ μ²λ¦¬νλ λ°©μμ **Batching** μ΄λΌκ³  νλ©°, μ΄ λμ React μμλ λΆνμν λ¦¬λ λλ§μ λ°©μ§ν  μ μκ² λμλ€.
- λν Batching μμμ ν΅ν΄ μ»΄ν¬λνΈ λ΄λΆμ μΌλΆ state update μμμ΄ μ€νλμ§ μμ μνλ‘ λ¦¬λ λλ§μ΄ μ§νλλ **half-finished** μνλ₯Ό λ§μ μλ μλ€.

```jsx
function changeState() {
	const [flag, setFlag] = useState(false);
	// setFlag (setter) ν¨μκ° μΈ μ°¨λ‘ μ€νλμμ§λ§, λ¦¬λ λλ§μ λ¨ νλ²λ§ λ°μνκ² λλ€.
	setFlag((prevFlag) => !prevFlag); // false => true
	setFlag((prevFlag) => !prevFlag); // true => false
	setFlag((prevFlag) => !prevFlag); // false => true
}
```

> **Batching** μ λν΄ λ μμΈν κΈ°μ ν ν¬μ€νμ μλμ μλ€.

https://velog.io/@rookieand/React-18%EC%97%90%EC%84%9C-%EC%B6%94%EA%B0%80%EB%90%9C-Auto-Batching-%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80

### βοΈ State is a snapshot

- React μμ μ»΄ν¬λνΈλ₯Ό λ λλ§νλ€λ μλ―Έλ ν¨μν μ»΄ν¬λνΈλ₯Ό νΈμΆνλ€λ κ²μ λμΌνλ€. λν ν¨μλ‘λΆν° λ°νλ JSX λ μ»΄ν¬λνΈκ° λ λλ§λ μκ°μ **μ€λμ·** μμ μ μνμ.
- λν Reactλ μ»΄ν¬λνΈ λ΄λΆμ props, μ΄λ²€νΈ νΈλ€λ¬, μ§μ­ λ³μλ₯Ό λ λλ§μ΄ μ§νλ μκ°μ stateλ₯Ό νμ©νμ¬ κ³μ°νκ³ , μ΄λ₯Ό μ΅μ’μ μΌλ‘ λ°ννκ² λλ€.
- stateλ μ»΄ν¬λνΈ μΈλΆμμ Closure ννλ‘ κ΄λ¦¬λλ λ³μμ΄λ€. React λ μ»΄ν¬λνΈ ν¨μκ° νΈμΆλ μκ°μ state κ° (μ€λμ·) μ μ κ³΅νλ©° μ΄λ λ€μ λ λλ§ μ΄μ κΉμ§ λ³νμ§ μλλ€.

```jsx
import { useState } from "react";

function Counter() {
	// μ΄κΈ° λ λλ§ μ count κ°μ 0μΌλ‘ κ³ μ λλ€. μ΄λ λ€μ λ λλ§ μ΄μ κΉμ§ μ λ λ³κ²½λμ§ μλλ€.
	const [count, setCount] = useState(0);

	function increaseCount() {
		// μ΄κΈ° λ λλ§ μ΄ν count κ°μ 0μΌλ‘ κ³ μ λλ€.
		setCount(count + 1); // 0 + 1 = 1
		setCount(count + 1); // 0 + 1 = 1, μ΄μ μ κ²°κ³Όμ μν₯μ λ°μ§ μκ³  νμ¬ count κ°μ μ μ©νλ€.
		setCount(count + 1); // 0 + 1 = 1, μ΄μ μ κ²°κ³Όμ μν₯μ λ°μ§ μκ³  νμ¬ count κ°μ μ μ©νλ€.
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

- μλ¨μ μ½λλ μ΄ μΈ λ²μ setState μμμ μννμ§λ§ λ¦¬λ λλ§ ν count κ°μ κΈ°λμ λ¬λ¦¬ `0` μμ `1` λ‘ λ³κ²½λλ€.
- νμ¬ `count` state κ°μ μ΄κΈ° λ λλ§μ μ§νν ν 0μΌλ‘ κ³ μ λμλ€. λ°λΌμ `setCount(count + 1)` μμμ κ²°κ³Όλ 0μ 1μ λν κ°μΌλ‘ stateλ₯Ό μλ°μ΄νΈ ν  κ²μ΄λ€.
- μλ¨μ μ½λμμλ μ΄ μμμ μΈ μ°¨λ‘ μννμμΌλ, `count` κ°μ νμ¬ 0μΌλ‘ κ³ μ λ μνκΈ°μ κ²°κ΅­ μ¬λ¬λ² μμμ μνν΄λ λ¦¬λ λλ§ μ΄ν κ²°κ³Όλ 1λ‘ λ³κ²½λ  κ²μ΄λ€.
- μ€μν κ²μ κ°κ°μ state update μμμ΄ νλλ‘ ν©μ³μ§μ§ μκ³  μμ°ν μμ°¨μ μΌλ‘ μ€νλμλ€λ μ μ΄λ€. Batching μ μλ°μ΄νΈ μμμ μΌκ΄μ μΌλ‘ **μ²λ¦¬νλ κ²** μ΄μ§, λμΌν μμμ νλλ‘ ν©μΉλ κ²μ μλλ€.

```jsx
import { useState } from "react";

function Counter() {
	// μ΄κΈ° λ λλ§ μ count κ°μ 0μΌλ‘ κ³ μ λλ€. μ΄λ λ€μ λ λλ§ μ΄μ κΉμ§ μ λ λ³κ²½λμ§ μλλ€.
	const [count, setCount] = useState(0);

	function increaseCount() {
		// λ¦¬λ λλ§ μ΄ν count κ°μ 3μΌλ‘ λ³κ²½λλ€. νμ¬ count κ°μ 0μμ μ μ
		setCount(count + 100); // 0 + 100 = 100
		setCount(count - 200); // 0 - 200 = -200
		setCount(count + 1); // 0 + 1 = 1, μ΅μ’μ μΌλ‘ count κ°μ 1λ‘ μμ λμ΄ λ€μ λ λλ§μ λ°μλ¨
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

- μλ¨μ μ½λλ μ΄ μΈ λ²μ setState μμμ μννμκ³ , λ¦¬λ λλ§ μ΄ν count κ°μ `1` λ‘ λ³κ²½λμλ€.
- μ¬κΈ°μλ μ΄κΈ° λ λλ§ μ count κ°μ΄ `0` μμ μ μν΄μΌ νλ€. μΈ λ²μ `setCount` ν¨μκ° νΈμΆλμμΌλ μΈμλ‘ λ°μ count κ°μ `0` μΌλ‘ κ³ μ λλ€.
- λ°λΌμ **κ°μ₯ λ§μ§λ§μ μ€νλ** `setCount` μ κ²°κ³Όκ° μ΅μ’μ μΌλ‘ λ³κ²½λ count κ°μ΄ λκ³ , λ¦¬λ λλ§ μ΄ν count κ°μ `1` μΌλ‘ μμ λλ κ²μ΄λ€.

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
					// setTimeout ν¨μλ₯Ό ν΅ν΄ λΉλκΈ°μ μΌλ‘ alert ν¨μλ₯Ό 3μ΄ ν νΈμΆμμΌ°μ
					// νμ§λ§ ν΄λΉ μμμ΄ μ€μΌμ€λ¬μ λ±λ‘λ λΉμμ state κ°μΈ 0μ΄ μ μ©λλ€.
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

- μλ¨μ μ½λλ λ²νΌμ ν΄λ¦­ν  κ²½μ° `number` state μ 5λ₯Ό λνκ³ , setTimeout ν¨μλ₯Ό μ¬μ©νμ¬ 3μ΄ νμ `number` stateλ₯Ό alert μ°½μ λμ°λλ‘ νμλ€.
- νμ§λ§ 3μ΄ ν alert μ°½μλ 5κ° μλ `0` μ΄ λμμ§λ€. μλνλ©΄ alert ν¨μμ μΈμλ‘ λ€μ΄κ° numberλ μ¬μ©μκ° **λ²νΌμ ν΄λ¦­νμ λμ state κ°**μ λ΄μκΈ° λλ¬Έμ΄λ€.
- μ€λ Ή alert ν¨μκ° λ¦¬λ λλ§ μ΄νμ μ§νλλλΌλ, ν΄λΉ μμμ΄ μ€μΌμ€λ¬μ λ±λ‘λ  λΉμμ state κ°μ **λ¦¬λ λλ§μ΄ μΌμ΄λκΈ° μ μ κ°**μ΄κΈ°μ μ΄λ° νμμ΄ λ°μνλ κ²μ΄λ€.
- κ²°κ΅­ state λ λ€μ λ λλ§μ΄ λ°μνμ§ μλ μ΄μ **νμ¬μ κ°μ μ μ§νλ€.** (λΆλ³νλ€) μ€λ Ή μ΄λ²€νΈ νΈλ€λ¬ λ΄λΆμ μ½λκ° λΉλκΈ°μ μΌλ‘ μλνλλΌλ λ§μ΄λ€.

### βοΈ updater function

- λ§μ½ setter ν¨μμ μΈμμ κ°μ΄ μλ ν¨μλ₯Ό λ£κ² λλ€λ©΄, React μμλ μ΄λ₯Ό **updater function** μΌλ‘ μΈμνκ² λλ€.
- React λ Batching μμμ ν΅ν΄ state update μμμ μΌκ΄ μννλλ°, λ§μ½ updater function μ΄ μΈμλ‘ λ€μ΄μλ€λ©΄ ν΄λΉ ν¨μμ return κ°μ λ€μ update μμμ μΈκ³νλ€.

```jsx
import { useState } from "react";

function Counter() {
	// μ΄κΈ° λ λλ§ μ count κ°μ 0μΌλ‘ κ³ μ λλ€. μ΄λ λ€μ λ λλ§ μ΄μ κΉμ§ μ λ λ³κ²½λμ§ μλλ€.
	const [count, setCount] = useState(0);

	function increaseCount() {
		setCount(prev => prev + 1); // 0 + 1 = 1, μ΄κΈ° λ λλ§ μ΄ν count κ°μ 0μΌλ‘ κ³ μ λλ€.
		setCount(prev => prev + 1); // 1 + 1 = 2, μλ¨μ updater functionμ return κ°μΈ 1μ μΈκ³ λ°μλ€.
		setCount(prev => prev + 1); // 2 + 1 = 3, μλ¨μ updater functionμ return κ°μΈ 2μ μΈκ³ λ°μλ€.

	return (
		<div>
			<button onClick={increaseCount}>+3</button>
			<p>Count : {count}</p>
		</div>
	);
}

export default Counter;
```

- μλ¨μ μ½λλ setter ν¨μμ update function μ μΈμλ‘ λκ²¨, μ΄μ  state update μμμ κ²°κ³Όλ₯Ό κΈ°λ°μΌλ‘ ν¨μλ₯Ό μ€νμν¨ λ€ λμ¨ λ°ν κ°μ λ€μ update function μΌλ‘ μΈκ³νμλ€.
- μ£Όμν  μ μ, update function μ λ°λμ **μμ ν¨μ** μ¬μΌ νκ³ , μ€μ§ λ³κ²½λ state κ°λ§μ return ν΄μΌ νλ€. ν¨μ λ΄λΆμ stateλ₯Ό μ¬μ€μ νκ±°λ λ€λ₯Έ Side Effectλ₯Ό μ€νμμΌμλ μλλ€.

```jsx
import { useState } from "react";

function App() {
	// μ΄κΈ° λ λλ§ μ count κ°μ 0μΌλ‘ κ³ μ λλ€. μ΄λ λ€μ λ λλ§ μ΄μ κΉμ§ μ λ λ³κ²½λμ§ μλλ€.
	const [count, setCount] = useState(0);

	function increaseCount() {
		// λ¦¬λ λλ§ μ΄ν count κ°μ 1μΌλ‘ λ³κ²½λλ€. νμ¬ count κ°μ 0μμ μ μ
		setCount(count + 100); // 0 + 100 = 100
		setCount((prevCount) => prevCount + 1); // 100 + 1 = 101
		setCount((prevCount) => prevCount + 1); // 101 + 1 = 102
		setCount(count + 1); // 0 + 1 = 1, μ΅μ’ count κ°μ 1μΌλ‘ λ³κ²½
	}

	return (
		<div>
			<button onClick={increaseCount}>+1</button>
			<p>Count : {count}</p>
		</div>
	);
}

export default App;
```

- ν κ°μ§ μ μν  μ μ, updater functionμ μμ  μμμ κ²°κ³Όλ₯Ό λ°μνμ¬ ν¨μλ₯Ό μ€ννμ§λ§, λ¨μν κ°μ μ λ¬νμ κ²½μ°μλ **μμ  μμμ κ²°κ³Όλ₯Ό λ¬΄μ** νκ³  νμ¬ state μ κ°λ§μ κ³ λ €νλ€λ κ²μ΄λ€.
- μλ¨μ μ½λ λ΄λΆμλ λ μ°¨λ‘ update function μ΄ μ€νλλ©° state κ°μ 3μΌλ‘ λ³κ²½νμ§λ§, μ΄νμ μμμμλ μ΄λ₯Ό κ³ λ €νμ§ μκ³  κΈ°μ‘΄μ state κ°μ κΈ°λ°μΌλ‘ μλ‘­κ² count state κ°μ μλ°μ΄νΈ νλ€.
- λ°λΌμ update function μ κ²½μ° μ΄μ  μμμ κ²°κ³Όλ₯Ό κΈ°λ°μΌλ‘ μλ‘μ΄ κ°μ μλ°μ΄νΈ νμ§λ§, λ¨μν κ°μ μΈμλ‘ λκ²Όμ κ²½μ°μλ μ΄μ  μμμ κ²°κ³Όλ₯Ό κ³ λ €νμ§ μλλ€λ μ μ μ μν΄μΌ νλ€.

> React μμ μΆμ²νλ updater function μ Name Convention

```js
setEnabled((e) => !e);
setLastName((ln) => ln.reverse());
setFriendCount((fc) => fc * 2);
```

- React μμλ νμ¬ setter ν¨μκ° μλ°μ΄νΈ νλ €λ state λ³μμ μ κΈμλ§μ λ°μ μΈμλ₯Ό μμ±νλ κ²μ μΆμ²νλ€.
- λ§μ½ μ‘°κΈ λ κ΅¬μ²΄μ μΈ λ€μ΄λ°μ μ°κ³  μΆλ€λ©΄, `prev` κ°μ μ λμ¬λ₯Ό λΆμ΄κ±°λ μλλ©΄ state λ³μμ μ΄λ¦μ κ·Έλλ‘ μ¬μ©ν΄λ λλ€κ³  νλ€.

# βοΈ Initial Value Lazy Loading

### βοΈ Avoiding recreating the initial state

- `useState` νμ μ¬μ©ν  λ μΈμλ‘ λ£μ΄μ€ κ°μ μ΄κΈ° λ λλ§ μμ React μ μ μ₯λκ³ , μ΄νμ λ λλ§μμλ μ΄κΈ°μ μ μ₯νλ κ°μ κ·Έλλ‘ μ¬μ©νλ€.
- νμ§λ§ μ΄κΈ° κ°μ ν¨μμ μ€νμΌλ‘ λ£μ΄μ€ κ²½μ° λ§€ λ λλ§λ§λ€ ν΄λΉ ν¨μλ νΈμΆλ  κ²μ΄λ©°, ν΄λΉ ν¨μμ μ°μ°μ΄ λΉμΈμ§μλ‘ λΉμ©μ λ­λΉλ λμ± μ¬ν΄μ§ κ²μ΄λ€.

```jsx
function MyApp({ Component, pageProps }: AppProps) {
	const [queryClient] = useState(
		new QueryClient({
			defaultOptions: {
				queries: {
					refetchOnReconnect: false,
					refetchOnWindowFocus: false,
				},
			},
		})
	);
	return (
		<QueryClientProvider client={queryClient}>
			<ReactQueryDevtools initialIsOpen={false} />
			<Provider>
				<GlobalStyle />
				<ThemeProvider theme={theme}>
					<ModalPortal />
					<Component {...pageProps} />
				</ThemeProvider>
			</Provider>
		</QueryClientProvider>
	);
}

export default MyApp;
```

- μλ¨μ μ½λλ react-queryμ queryClient μΈμ€ν΄μ€λ₯Ό μμ±νκ³  μ΄λ₯Ό Providerμ μ μ©νλ λ‘μ§μ λ΄κ³  μλ€. νμ§λ§ λ§€λ² ν΄λΉ μ»΄ν¬λνΈκ° μ€νλ  κ²½μ° μλ‘μ΄ μΈμ€ν΄μ€λ₯Ό μμ±νμ¬ λΉμ©μ΄ λ­λΉλ  μ μλ€.
- μμ§ν μμ²­λκ² μ μ ν μμλ μλλΌκ³  μκ°νλ€. νμ§λ§ νμκ° useStateμ initialValue μ μ½λ°± ν¨μλ₯Ό μ§μ΄λ£λ μ½λλ₯Ό μ²μ μ ν κ²μ΄ μλ¨μ μ½λμ κ°μ μν©μ΄μκΈ°μ μμλ‘μ μ±νμ νμλ€.

```jsx
function MyApp({ Component, pageProps }: AppProps) {
	const [queryClient] = useState(
		() =>
			new QueryClient({
				defaultOptions: {
					queries: {
						refetchOnReconnect: false,
						refetchOnWindowFocus: false,
					},
				},
			})
	);
	return (
		<QueryClientProvider client={queryClient}>
			<ReactQueryDevtools initialIsOpen={false} />
			<Provider>
				<GlobalStyle />
				<ThemeProvider theme={theme}>
					<ModalPortal />
					<Component {...pageProps} />
				</ThemeProvider>
			</Provider>
		</QueryClientProvider>
	);
}

export default MyApp;
```

- νμ§λ§ λ€μκ³Ό κ°μ΄ μ½λ°± ν¨μλ‘μ μΈμλ₯Ό λ£μ΄μ£Όκ² λλ©΄, Reactμμλ μ΄κΈ° λ λλ§ μμλ§ ν΄λΉ ν¨μλ₯Ό νΈμΆνκ³  λ°νλ κ°μ **λ³λλ‘ μ μ₯νλ€**. μ΄νμ λ λλ§μ λν΄μλ μ μ₯λ κ°μ κΊΌλ΄μ μ μ©νλ λ°©μμΌλ‘ λΉμ©μ μ μ½ν  μ μλ€.
- λ§μ½ state μ μ΄κΈ° κ°μ μμ±νκΈ° μν ν¨μμ λΉμ©μ΄ ν¬λ€λ©΄, useState νμ `initialValue` μ ν¨μλ₯Ό μ€νμν€μ§ λ§κ³  ν¨μ μμ²΄λ₯Ό μΈμλ‘ λ£μ΄λ³΄μ. μ΄κΈ° λ λλ§ μμλ§ ν¨μκ° νΈμΆλκ³  μ΄νμλ λ°νλ κ°μ μ μ₯νμ¬ νμ©νλ ν¨μ¬ μ’λ€.

# π References

- https://beta.reactjs.org/learn/state-a-components-memory
- https://beta.reactjs.org/learn/state-as-a-snapshot#rendering-takes-a-snapshot-in-time
- https://beta.reactjs.org/learn/queueing-a-series-of-state-updates
- https://beta.reactjs.org/reference/react/useState#avoiding-recreating-the-initial-state
- https://github.com/reactwg/react-18/discussions/5
- https://seokzin.tistory.com/entry/React-useState%EC%9D%98-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC%EC%99%80-%ED%81%B4%EB%A1%9C%EC%A0%80
