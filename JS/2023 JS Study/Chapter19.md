# ✒️ Async / Await

### Async

- 기존의 Promise 문법을 `async`, `await` 키워드를 사용하여 보다 **편리하게** 사용할 수 있다.
- `async` 키워드는 항상 함수 앞에 붙으며, 해당 키워드가 붙은 함수는 항상 Promise를 반환한다.
- 만약 기존에 Promise를 반환하지 않았던 함수이더라도, 이행 (Resolved) 상태의 Promise로 값을 감싸 반환한다.

```javascript
async function f() {
	return 1;
}

f().then((data) => console.log(data)); // 1
```

### Await

- `await` 키워드는 항상 `async` 키워드가 붙은 함수 내부에서만 사용해야 한다.
- `await` 키워드가 붙을 경우 해당 작업은 반드시 Promise를 반환해야 한다.
- JS 는 `await` 키워드를 만나면 Promise가 이행되기 전까지 대기하며, 처리 이후에 실행이 재개된다.

> await는 최상위 레벨 코드에서 작동하지 않는가?

- ES2022 에서는 await 을 독립적으로 사용 가능하도록 문법이 개선되었다.

```javascript
async () => {
    await fetch('http://....');
};

// 개선된 문법
() => {
    await fetch('http://...');
}
```

- Promise가 처리되기를 기다리는 동안 JS 엔진은 다른 업무를 할 수 있기에 CPU 리소스를 절약할 수 있다.
- `Promise.then` 보다 더욱 가독성이 좋은 코드를 만들 수 있으며 쓰기도 쉽다.

```javascript
function walk(amount) {
	return new Promise((resolve, reject) => {
		if (amount < 500) {
			reject("value is too small");
		}
		setTimeout(() => resolve(`walk for ${amount} m`), amount);
	});
}

// 기존의 Promise Chaining을 활용한 방식
walk(1000)
	.then((res) => {
		console.log(res);
		return walk(500);
	})
	.then((res) => {
		console.log(res);
		return walk(700);
	})
	.then((res) => {
		console.log(res);
		return walk(800);
	});

// 비동기 함수 go를 만들고, async / await 구문으로 축약한 모습
async function go() {
	let res = await walk(1000);
	console.log(res);
	res = await walk(500);
	console.log(res);
	res = await walk(700);
	console.log(res);
	res = await walk(800);
	console.log(res);
}
```

### Catching Error

- `await` 가 던진 에러는 `try - catch` 블럭을 활용해서 캐치할 수 있다.
- Promise가 거부되었을 경우 `await` 구문은 `throw` 문을 작성한 것처럼 에러를 던진다.

```javascript
// 기본적으로는 try - catch 블록으로 에러를 캐치한다.
async function f() {
	try {
		let res = await fetch("http://testAPI....");
	} catch (err) {
		console.log(err);
	}
}

async function f() {
	let res = await fetch("http://testAPI....");
}

// 거부 상태의 Promise가 될 경우 catch 핸들러로 에러 캐치.
f().catch(alert);
```
