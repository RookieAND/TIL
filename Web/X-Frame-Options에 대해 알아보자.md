## X-Frame-Options

- X-Frame-Options HTTP 응답 헤더는 해당 브라우저가 `<frame>`, `<iframe>`, `<embed>` ,`<object>` 태그를 사용한 렌더링을 허용할지에 대한 여부를 알릴 때 쓰인다.
- 보통  [click-jacking](https://developer.mozilla.org/en-US/docs/Web/Security/Types_of_attacks#click-jacking) 이라 불리는 공격 수단으로 `<iframe>` 태그를 사용하는데, 이러한 문제를 방지하기 위해 **X-Frame-Options** 헤더 옵션을 추가하여 iframe 렌더링을 방지한다.
- edu-core 에서는 앱 최상단 (app.js) 내에서 직접 Response 객체 내의 header 속성에 **X-Frame-Options** 헤더를 삽입하는 방식으로 문제를 해결하였다.

```jsx
app.use((_req, res, next) => {
	res.removeHeader('x-powered-by');
	res.setHeader('X-Frame-Options', 'SAMEORIGIN');
	next();
});
```

- **X-Frame-Options HTTP Response** 헤더에는 총 3가지 옵션이 존재한다.
    - DENY : 해당 페이지는 `<iframe>` 을 활용한 페이지 렌더링을 일체 허용하지 않는다는 의미이다.
    - SAMEORIGIN : 같은 Origin 내에서의 참조만 허용하겠다는 의미이다. 즉 다른 Origin 에서 요청한 참조에 대해서는 거부한다.

## X-Powered-By

- **X-Powered-By** 는 서버 구축에 쓰인 프레임워크 혹은 프록시 서버에 대한 정보를 노출시키는 HTTP 응답 헤더이다.
    - **nginx, apache tomcat** 같은 프록시 서버 혹은 PHP 로 구축한 서버 등, 기본적으로 서버를 구축할 때 쓰인 기술 스택이 노출되기 때문에 보안 상 좋지 못하다.
    - edu-core 에서는 모든 요청에 대한 Response 객체 내의 **X-Powered-By** 헤더를 수동으로 제거함으로서 서버의 정보를 노출시키지 않고 있다.

## X-Forwarded-Proto / X-Forwarded-Port

- **X-Forwarded-Proto** HTTP 응답 헤더의 경우 현재 클라이언트가 서버와 연결하기 위해 사용한 프로토콜 (HTTP, HTTPS) 에 대한 정보를 담는다.
- **X-Forwarded-Port** HTTP 응답 헤더의 경우, 클라이언트가 서버와 연결하기 위해 사용한 포트에 대한 정보를 담는다.
- edu-core 에서는 HTTP 프로토콜로 서버에 접근했을 경우, 안전하지 않은 연결로 간주하고 HTTPS 프로토콜로 재접근하도록 사용자를 redirect 시킨다.

```jsx
var check_secure = function (req, res, next) {
	if (req.headers && req.headers['x-forwarded-proto'] === 'https') {
		req.connection.encrypted = true;
	}
	if (
		DASHBOARD_SECURE &&
		req.headers &&
		req.headers['x-forwarded-port'] === '80' &&
		req.headers['x-forwarded-proto'] === 'http'
	) {
		res.redirect(`https://${req.headers.host}${req.originalUrl}`);
	} else {
		next();
	}
};
```