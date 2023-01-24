# React Testing Library (RTL)

#### What is RTL?

-   RTL은 JSX로 작성된 컴포넌트를 가상 DOM으로 렌더링해준다. (render)
-   RTL은 렌더링 된 가상 DOM을 일정한 기준에 맞춰 탐색해준다. (screen.getByText)
-   RTL은 렌더링된 가상 DOM 과 상호 작용 하는 것을 돕는다.
-   RTL의 경우 주로 Functional Test 에 사용하는 것을 권장함.

#### RTL은 어떤 기능을 하는가?

-   테스트를 위한 가상 DOM을 생성하고 DOM과 상호 작용하기 위한 유틸리티를 제공.
-   브라우저 없이도 React 관련 테스트를 진행할 수 있게 해준다.

#### 접근성과 요소를 찾는 방식

> https://testing-library.com/docs/queries/about/#priority

-   테스팅 라이브러리의 경우 접근성으로 요소를 찾을 수 있는 방식으로 요소를 찾기를 바람.
-   가상 DOM 에서 특정 요소를 찾을 경우, 아래와 같은 우선순위로 요소를 찾도록 유도.

1. 누구나 접근 가능한 쿼리

-   보조적인 기술을 사용하거나 마우스를 사용하며 화면을 시각적으로 바라보는 이들에게 적용. (평범하게 기기를 사용하여 웹 사이트를 이용하는 사람)
-   각 Element는 Document에서 역할을 가진다. (button, heading 등)
-   또한 HTML Element에 사용자가 직접 role 요소를 추가해줄 수도 있다.
-   `getByRole` : 페이지를 이루는 요소의 역할을 식별하도록 한다.
-   `getByLabelText` : 폼 필드에 유용하여 label 텍스트로 요소를 찾는다.
-   `getByPlaceholderText` : `input` 태그의 placeholder 텍스트를 대상으로 요소를 찾는다.
-   `getByText` 의 경우 디스플레이 요소 (`div`, `p`, `span`) 를 찾기 위해 쓰인다.

2. 시멘틱 쿼리

-   `getByAltText` : `img`, `area`, `input` 같이 alt 텍스트를 지원할 경우 이를 통해 요소를 찾음.
-   `getByTitle` : title 속성의 경우 스크린 리더에 지속적으로 읽히지 않기 때문에 사용을 지양.

3. 테스트 ID

-   `getByTestId` : Test ID는 사용자가 볼 수도 없고, 스크린 리더 또한 접근이 불가하기에 최후의 수단으로 써야 함.

> Testing Library에서 Query 란?

-   Query는 테스팅 라이브러리가 페이지 내에서 특정 요소를 찾기 위해 제공하는 메서드이다.
-   쿼리를 통해 요소를 선택했다면, Event API를 통해 이벤트를 실행하고 사용자의 인터렉션을 시뮬레이션 할 수 있다.
-   혹은 Jest를 활용하여 특정 요소에 대한 Assertion을 진행할 수도 있다.

#### CRA의 App.test.js를 접근성 측면에서 재수정하자.

```javascript
import { render, screen } from '@testing-library/react';
import App from './App';

test('renders learn react link', () => {
    render(<App />);
    // 텍스트만을 사용하여 요소를 찾는 경우 접근성 측면에서 떨어짐.
    const oldLinkElement = screen.getByText(/learn react/i);
    // anchor 태그는 기본적으로 link role을 내장함, 따라서 getByRole로 요소 탐색 가능.
    const linkElement = screen.getByRole('link', { name: /learn react/i });
    expect(linkElement).toBeInTheDocument();
});
```
