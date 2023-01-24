# Testing Library & Jest란?

```javascript
import { render, screen } from '@testing-library/react';
import App from './App';

test('renders learn react link', () => {
    render(<App />);
    const linkElement = screen.getByText(/learn react/i);
    expect(linkElement).toBeInTheDocument();
});
```

```javascript
import logo from './logo.svg';
import './App.css';

function App() {
    return (
        <div className='App'>
            <header className='App-header'>
                <img src={logo} className='App-logo' alt='logo' />
                <p>
                    Edit <code>src/App.js</code> and save to reload.
                </p>
                <a className='App-link' href='https://reactjs.org' target='_blank' rel='noopener noreferrer'>
                    Learn React
                </a>
            </header>
        </div>
    );
}

export default App;
```

-   RTL (react-testing-library) 에서는 render와 screen 객체를 지원.
-   render 함수는 인자로 받은 JSX를 기반으로 가상 DOM을 생성함.
-   screen 전역 객체는 렌더링된 가상 DOM에 접근할 수 있도록 해줌.
-   `screen.getByText` 메소드는 인자로 받은 정규식 혹은 문자열을 만족하는 텍스트가 포함된 Element를 찾음
-   테스트 대상인 App 컴포넌트 내부에 `Learn React` 텍스트를 가진 Element가 있으므로, 테스트는 통과하게 됨.

# Assertion

```javascript
expect(linkElement).toBeInTheDocument();
```

#### Assertion 이란?

-   프로그래밍 관점에서 Assertion (단언) 이란, 해당 지점에서 개발자가 반드시 `true (참)` 이어야 한다고 생각하는 사항을 표현한 논리식이다.
-   Assertion이 위반되는 경우는 프로그램 내에 버그나 기타 문제가 있음을 암시한다.

#### Jest 내의 expect, matcher 란?

-   `expect` 함수는 Jest의 전역 메서드이며, 테스트의 통과 여부를 결정하는 Assertion 의 시작입니다.
-   `expect` 함수의 인자는 Assertion을 검증하기 위한 요소로, 예측이 맞는지를 확인하기 위해 Jest에서 요구하는 값이다.
-   `matcher` 는 Assertion의 유형이며, 다양한 종류의 matcher를 지원한다. 요소를 받는 경우도 있고 아닌 경우도 있다.
    -   `toBeInTheDocument()` 는 `expect` 함수로 받은 요소가 Document 내부에 있는지를 검증한다. (Jest-DOM)
    -   `toBe()` 는 expect 함수로 받은 요소가 matcher에서 받은 인자와 동일한지를 검증한다.
    -   `toHaveLength()` 는 expect 함수로 받은 배열의 길이가 mathcer에서 받은 인자와 동일한지를 검증한다.

# Jest

#### What is Jest?

-   사용자가 제작한 테스트를 찾고, 이를 실행하며 Assertion을 생성하는 라이브러리다.
-   create-react-app 설치 시 자동으로 제공되며, Testing Library에서 사용을 장려하고 있다.

#### Jest Watch Mode

-   react-script의 test 명령어 실행 시, Jest Watch Mode가 실행됨
-   Jest를 실행하는 여러 방법 중 하나이며, 마지막 커밋 이후 변경된 파일과 연관된 테스트만을 자동으로 실행시킴.
-   만약 변화가 없을 경우에는 사용자가 전체 테스트를 진행하기 전까지는 어떠한 테스트도 진행하지 않음.
-   실제로 Watch Mode를 실행한 후 파일을 수정하면, 수정된 파일과 연관된 테스트가 자동으로 실행됨.

#### How does Jest Work?

```javascript
test('test name', () => {
    const testElm = 2;
    expect(testElm).toEqual(1); // failed
});

test('empty test', () => {}); // success
test('throw Error' () => {
	throw new Error('test failed') // failed
})
```

-   Jest에서는 2개의 인자를 가진 test 전역 메소드를 사용함.
-   첫번째 인자는 테스트의 식별자를 지정, 다수의 테스트를 실행할 경우 각 테스트의 통과 유무 파악 가능.
-   두번째 인자는 테스트 함수이며, 실행 도중 에러가 발생한다면 테스트는 실패 처리됨.
-   Assertion 의 기댓값과 실제 결과가 다를 경우 자동으로 에러를 발생시킴. 테스트 실패 처리.
-   에러가 없다면 자동으로 테스트는 통과되므로, 빈 테스트의 경우에도 통과됨.

#### Jest-Dom

-   CRA (creact-react-app) 설치 시 자동으로 패키지에 포함되어 설치된다.
-   `src/setupTests.js` 에서 매번 테스트를 진행할 때마다, `@testing-library/jest-dom` 을 import 해온다.
-   `toBeVisible()`, `toBeChecked()` 같이 가상 DOM에서 지원하는 matcher 들을 사용하도록 해준다.
