# ✒️ Trouble-Shooting

- 서버 개발자 분이 배포하신 서버로 리소스를 요청하였고, 정상적으로 네트워크 탭에 `Set-Cookies` 헤더와 `Authorization` 헤더가 담겨져서 온 것을 확인했다.
- 하지만 이후의 요청에 쿠키가 헤더에 담겨져서 전송되지 않았고, `withCredentials` 옵션을 **true** 로 설정해도 서버로부터 쿠키를 받아 요청을 보내고 값이 담기지 않는 문제가 발생하였다.
- 찾아보니 2020년 부터는 Chrome 에서 **다른 출처 (Origin)** 에서 온 쿠키, 즉 서드 파티 쿠키에 대해서는 오직 **SameSite** 쿠키의 값이 `None` 인 경우에만 쿠키의 전송을 허가한다는 추가적인 보안 규칙을 세웠다.
- **SameSite** 쿠키의 값을 None 으로 설정하려면 HTTPS 연결인 경우에만 허용되기 때문에, HTTP 를 사용 중인 현재 상황에서는 쿠키를 사용하기가 어렵다는 결론을 내렸다. 아니면 서브 도메인을 만들던가..

# ✒️ Origin

- 서버의 위치를 의미하는 URL 의 구성요소 중에서 **Protocol** 과 **host**, 마지막으로 **Port** 번호를 모두 합친 것을 Origin 이라고 한다. 이 중에서 **하나라도 다르다면** 둘은 서로 **다른 출처를 가진다고** 브라우저에서 판단한다.
- 그 외에 다른 요소의 경우는 아무리 달라도 상단에서 언급한 세 개의 요소가 동일하다면 같은 출처라고 판단한다.
- 아래 표는 URL `http://store.company.com/dir/page.html`의 출처를 비교한 예시다.
    
    
    | URL | 결과 |
    | --- | --- |
    | http://store.company.com/dir2/other.html | 성공 |
    | http://store.company.com/dir/inner/another.html | 성공 |
    | https://store.company.com/secure.html | 실패 (Protocol 이 다름) |
    | http://store.company.com:81/dir/etc.html | 실패 (Port 가 다름) |
    | http://news.company.com/dir/other.html | 실패 (Host 가 다름) |
- 만약 출처에 포트 번호가 명시적으로 포함되어 있다면 **이 또한 같아야** 동일한 출처라고 인정 된다.
- 하지만 해당 케이스의 경우 표준으로 정해진 것이 아니기 때문에, 브라우저에 따라서 동작이 달라진다.
    - 따라서 [http://localhost:3000](http://localhost:3000) 와 [http://localhost:8080](http://localhost:8080) 도 다른 출처이다. (포트 번호가 다르기 때문)
    - 현재 IE 를 제외한 나머지 브라우저들은 모두 포트 번호를 체크한다. IE 는 이제 좀 무시하자..

# ✒️ Same-Origin Policy (SOP)

- 동일 출처 정책 **(Same-Origin Policy)** 은 **같은 출처에서만** 리소스를 공유할 수 있다는 규칙을 가진 정책이다.
- 하지만 웹의 경우 다른 출처에 있는 리소스를 가져와서 사용하는 일이 많기 때문에 이를 전부 막기란 어려웠다.
- 따라서 몇 가지 예외 사항을 두었는데 그 중 하나가 바로 **“CORS 정책을 지킨 리소스 요청”** 이다.
    - 그 외 다른 출처의 이미지를 렌더링 하거나, 스타일 시트를 적용하거나, 스크립트를 실행하는 작업도 허용된다.
    - 따라서 SOP 를 지키지 않았는데, CORS 정책까지 마저 지키지 않았다면 당연히 리소스 요청이 불가능하다.

# ✒️ CORS (Cross-Origin Resource Sharing)

- **교차 출처 리소스 공유 (Cross-Origin Resource Sharing)** 란, 추가적인 HTTP header 를 사용하여 한 출처에서 실행 중인 웹 애플리케이션이 **다른 출처의 리소스**에 **접근할 수 있는 권한을 부여**하도록 브라우저에게 알리는 정책을 의미한다.
- 보안 상의 이유로 브라우저는 SOP 에 의거하여 다른 출처의 HTTP 요청을 제한하지만, 올바른 CORS 헤더를 포함한 응답을 반환하고, 해당 출처에서 접근을 허용했다면 CORS 정책을 지킨 요청으로 처리된다.

### 📒 CORS 의 동작 방식

- 기본적으로 HTTP 프로토콜을 사용하여 요청을 보낼 때는 헤더의 Origin 필드에 요청을 보내는 출처를 담아 보낸다.
- 이후 서버에서는 응답 헤더 내 `Access-Control-Allow-Origin` 필드에 접근이 허용된 Origin 을 담아 보낸다.
- 응답을 받은 브라우저는 자신이 보냈던 요청 헤더의 `Origin`  값과 서버가 보낸 응답 헤더의 `Access-Control-Allow-Origin` 값이 같은지를 비교하여 유효한 응답인지를 판별한다.
- 기본적인 골자는 이러하지만, 실제 서비스 상에서는 총 **세 가지 시나리오**가 존재하며 각각의 시나리오에 따라 변경된다.

### 📒 CORS 정책은 오직 브라우저에서만 동작한다

- 중요한 것은 출처를 비교하는 로직은 오직 **브라우저에서만** 구현되어 있는 스펙이기 때문에, 아무리 서버가 정상적으로 응답하여 리소스를 전송해도 브라우저 단에서 해당 요청이 CORS 정책에 위반된다고 판단한다면 **이를 버린다.**
- 따라서 코드 레벨 단에서는 CORS 에러가 터지더라도 응답은 정상적으로 수행이 되었기 때문에 **에러를 throw 하지 못한다.** 브라우저 구현 스펙에 포함되었기 때문에 서버 간의 통신에서는 해당 정책이 적용되지 않기 때문이다.
- 만약 Preflight 요청에서 서버로부터 허용된 Origin 값이 요청으로 보낸 Origin 값과 다르다면 브라우저는 CORS 정책 위반으로 처리하지만 응답은 정상적으로 처리된다. 이는 Preflight 요청에 대한 응답을 받은 이후에 판단되기 때문에 **CORS 에러는 요청의 성공 여부와 관련이 없다.**

### 📒 Preflight Request

- **Preflight Request** 란, 브라우저에서 요청을 한번에 보내지 않고 먼저 예비 요청을 보내어 접근 허용 여부를 미리 테스트 하는 방식이다.
- 이때 브라우저가 본 요청을 보내기 전에 `OPTIONS` HTTP Method 를 통해서 보내는 예비 요청을 **Preflight** 라고 하며, 이를 통해 브라우저는 사전에 해당 요청이 정말로 안전한지를 확인한다.

![https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS/preflight_correct.png](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS/preflight_correct.png)

- 브라우저는 **Preflight** 요청 내의 헤더에 실제 요청에서 **어떤 헤더와 어떤 HTTP Method 를 사용할지**를 알린다.
    - `Access-Control-Request-Headers` : 본 요청 전송 시 어떤 HTTP Method 로 전송할 것인지를 알림.
    - `Access-Control-Request-Method` : 본 요청 전송 시 어떤 사용자 정의 헤더가 전송될 것인지를 알림
- 서버는 응답 헤더에 사용 **가능한 HTTP Method 와  Header 목록**을 전달한다. 이후 브라우저에서는 이전에 보냈던 요청과 이를 대조하여 유효한 응답인지를 대조한다.
    - `Access-Control-Allow-Origin` : 리소스를 가져올 수 있는 출처
        - `*` 으로 설정할 경우, 모든 출처에 대한 리소스를 허용한다는 의미다.
    - `Access-Control-Allow-Methods` : 리소스에 접근할 시 허용되는 HTTP 메서드 목록
    - `Access-Control-Allow-Headers` : 리소스에 접근할 시 허용되는 사용자 지정 헤더 목록
    - `Access-Control-Max-Age` : Preflight 요청에 대한 결과를 캐싱하는 시간
        - 해당 시간 이내에 같은 리소스에 대한 접근을 재차 시도할 경우, Preflight 요청을 생략하고 캐싱된 결과를 토대로 본 요청 진행

```
OPTIONS /resources/post-here/ HTTP/1.1
Host: bar.other
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.14; rv:71.0) Gecko/20100101 Firefox/71.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Connection: keep-alive
Origin: http://foo.example
Access-Control-Request-Method: POST
Access-Control-Request-Headers: X-PINGOTHER, Content-Type

HTTP/1.1 204 No Content
Date: Mon, 01 Dec 2008 01:15:39 GMT
Server: Apache/2
Access-Control-Allow-Origin: https://foo.example
Access-Control-Allow-Methods: POST, GET, OPTIONS
Access-Control-Allow-Headers: X-PINGOTHER, Content-Type
Access-Control-Max-Age: 86400
Vary: Accept-Encoding, Origin
Keep-Alive: timeout=2, max=100
Connection: Keep-Alive
```

- 상단의 예시 요청과 응답에서, 클라이언트 에서는 이후 본 요청에 어떤 헤더와 어떤 메서드를 사용할지 알렸다.
    - `Access-Control-Request-Headers` : POST
    - `Access-Control-Request-Method` : X-PINGOTHER, Content-Type
- 이후 서버 측에서는 응답 헤더 내에 허용 가능한 출처와 HTTP Method 목록, 헤더 목록을 담아 보내주었다.
    - `Access-Control-Allow-Origin` : http://foo.example
    - `Access-Control-Allow-Methods` : POST, GET, OPTIONS
    - `Access-Control-Allow-Headers` : X-PINGOTHER, Content-Type
    - `Access-Control-Max-Age` : 86400
- 이전 요청에서 의뢰한 POST Method 도 허용되며 `X-PINGOTHER`, `Content-Type` 헤더도 허용하기 때문에 CORS 정책에 위반되지 않는 요청임을 확인했다. 이후 본 요청을 통해 정상적으로 리소스를 가져올 수 있게 되었다.

### 📒 Simple Request

- 일부 요청은 **Preflight** 요청을 선행하지 않고 즉시 다른 출처의 리소스에 대한 요청을 보내는데, 이를 **Simple Request** 라고 MDN 에서는 정의하였다.
- 모든 조건에서 **Preflight** 요청을 생략할 수 있는 것은 아니다. 아래의 조건이 충족되는 경우에만 Simple Request 요청이 가능하다.
    - HTTP Method 가 **GET, HEAD, POST** 중 하나인 경우
    - `Accept`, `Accept-Language`, `Content-Language`, `Content-Type` 같이 CORS 로부터 안전하다고 정의된 헤더만 사용해야 한다. ([https://fetch.spec.whatwg.org/#forbidden-header-name](https://fetch.spec.whatwg.org/#forbidden-header-name))
    - `Content-Type` 의 경우 Media Type이 `application/x-www-form-urlencoded`, `multipart/form-data`, `text/plain` 인 경우에만 허용된다.
- 이 경우 사용자 인증에 쓰이는 `Authorization` 헤더라던가, 상당 부분에서 쓰이는 Media Type 인 `application/json` 도 사용이 불가능하기 때문에 극히 일부 케이스에서만 쓰인다.

### 📒 Request with Credentials

- 브라우저에서 제공하는 비동기 리소스 요청 API 인 **XMLHttpsRequest** 나 **fetch API** 는 별도의 옵션 없이 브라우저의 쿠키 정보나 인증 관련 헤더를 요청에 담지 않는다.

![https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS/cred-req-updated.png](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS/cred-req-updated.png)

- 만약 이를 요청에 담아 보내고 싶다면  `credentials` 옵션을 별도로 설정해주어야 한다.
    - **same-origin** : 동일 출처 내에서만 인증 정보를 담을 수 있도록 한다. (기본 값)
    - **include** : 모든 요청에 대해서 인증 정보를 담을 수 있도록 한다.
    - **omit** : 모든 요청에 대해서 인증 정보를 담을 수 없도록 설정한다.
- **XMLHttpRequest** 의 경우에는 `withCredentials` 옵션을 통해서 다른 출처의 요청에 대해서 인증 정보를 담도록 설정할 수 있다. 같은 출처의 요청에 대해서는 영향을 미치지 않는다.
- 그리고 나서 응답 헤더 내에 `Access-Control-Allow-Credentials` 필드 값이 반드시 **true** 로 설정되어야 한다. 클라이언트에서 credentials 설정을 마쳐도 서버에서 해당 필드를 설정해주지 않으면 인증 정보를 담지 못한다.
- 만약 인증 정보를 허용한다면, `Access-Control-Allow-Origin`  헤더에 와일드카드 (*) 를 넣을 수 없고, 반드시 고정된 값을 넣어야 한다.

### 📒 이러한 정책이 필요한 이유

- 사실 출처가 다른 두 어플리케이션 간의 소통이 제약 없이 가능하다는 것은 그만큼 보안에 취약한 환경임을 의미한다.
- 당장 브라우저의 개발자 도구를 통해서 현재 사이트의 DOM 구조라던가, 통신하고자 하는 서버의 출처라던가, JS 코드들도 제약없이 확인할 수 있다. JS 코드는 난독화가 되었다지만 결국 변수 명을 단순하게 변경하고 indent를 축약한 방식이기 때문에 코드의 구조까지는 숨기기 어렵다.
- 이런 상황에서 악성 유저가 **CSRF (Cross-Site Request Forgery)** 나 **XSS (Cross-Site Scripting)** 같은 방법으로 실제 어플리케이션에서 코드가 실행된 것처럼 악성 코드를 꾸민다면 개인 정보를 쉽게 탈취당할 수 있다.
- 따라서 다른 출처 간의 리소스를 공유하는 과정에서는 이 요청이 정말로 안전한지, 그리고 서버에서 인증된 출처로부터 온 요청인지를 반드시 검증하는 과정이 필요하다.