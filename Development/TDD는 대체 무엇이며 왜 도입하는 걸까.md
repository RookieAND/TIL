# ✒️ TDD (Test Driven Development)

#### TDD (테스트 주도 개발) 는 무엇인가?

-   코드 작성 전에 테스트를 작성하고, 이 테스트에 통과하도록 코드를 작성하는 것이다.
-   애자일 방법론 중 하나인 eXtream Programming의 `Test-First` 개념에 기반을 둔 단순 설계를 강조한다.

#### TDD 의 개발 주기는 어떻게 되는가?

-   TDD 방식으로 코드를 작성하기 위해서는 아래의 과정을 거친다.
-   이를 **red-green 테스트** 라고도 하는데, 코드 작성 전에 실패하는 테스트 코드 `(레드)` 를 작성하고, 코드 작성 후에는 테스트가 통과 `(그린)` 하는지를 확인하는 것.

```
1. 코드를 작성하기 전에 테스트에 오류가 발생하지 않도록 최소한의 코드를 먼저 작성한다.
2. 이후 기능을 정상적으로 구현했다면 테스트가 통과되도록 테스트 코드를 작성한다.
3. 이후 작성된 테스트를 실행하고 실패를 확인한다. 현재 코드는 최소한의 양만 작성되었기에 당연히 실패한다.
4. 이후 테스트에 통과할 수 있도록 관련 코드를 작성하고, 테스트를 통과시킨다.
```

-   중요한 것은 실패하는 테스트 코드를 작성하기 전까지는 실제 코드를 작성하지 않아야 한다.
-   또한 실패하는 테스트를 통과할 수 있을 정도의 실제 코드를 최소한 작성해야 한다.

### 일반적인 개발 방식과는 어떤 차이가 있는가?

1. 일반적인 개발 방식

-   보통 개발 방식은 **요구사항 분석 - 설계 - 개발 - 테스트 - 배포** 순으로 이루어진다.
-   하지만 위 경우 초기에 완벽한 설계를 하는 것이 어려우므로, 잠재적인 버그나 추가 요구 사항으로 인한 재설계를 진행한다.
-   재설계를 진행하며 개발자는 코드를 추가하거나 수정하게 되는데, 이 과정에서 불필요한 코드가 추가될 가능성이 높다.
-   결국 코드의 재사용성이 떨어지고, 관리가 어려워지며 유지보수를 어렵게 하는 원인이 된다.
-   작은 파트의 기능 수정에도 애플리케이션의 모든 부분에 대한 테스트가 진행되어야 하기에 버그의 원인을 찾기가 어려워진다.

2. TDD 개발 방식

-   TDD 개발 방식의 경우 테스트 코드를 사전에 작성한 후 실제 개발 코드를 작성한다는 것이다.
-   따라서 설계 과정에서 프로그래밍 목적을 반드시 정의하고, 무엇을 테스트해야 할지도 미리 정해야 한다.
-   테스트 코드를 작성하면서 발생하는 예외 사항의 경우 테스트 케이스에 추가하여 설계를 개선한다.
-   이후 개발을 진행하며 테스트에 통과된 코드들만 실제로 사용하기에, 버그가 줄며 코드가 간결해진다.

### 왜 TDD를 채용해야 하는가?

1. 테스트를 작성하는 과정이 개발 프로세스의 한 부분을 차지한다.

    - 기존의 개발 과정은 테스트를 기능 구현 이후 마지막으로 진행하는 과정으로 여긴다.
    - 하지만 TDD 방식의 경우, 테스트 코드를 먼저 작성하고 이것을 통과시키기 위해 코드를 작성한다.
    - 따라서 코드를 성공적으로 작성하여 기능 구현을 완료했을 경우, 테스트를 번거로운 일처럼 느끼지 않게 된다.

2. 개발에서의 효율성이 증가한다.

    - 기존 방식의 경우, 매번 애플리케이션을 실행하면서 원하는대로 작동을 하는지 확인하며 소프트웨어를 업데이트 한다.
    - 하지만 기능 구현 전에 테스트 코드를 작성하면, 코드를 변경할 시 그와 관련된 테스트가 자동으로 실행된다.
    - 따라서 개발을 진행하면서 모든 테스트를 작성해두면 변경 사항이 발생할 때마다 이를 재실행하며 자동으로 테스트를 진행할 수 있다.
    - 이는 기존의 수동적인 테스트 방식과는 달리, 매번 서비스를 실행하고 직접 작동하는지 확인할 필요가 없다.

# ✒️ 테스트의 종류

1. Unit Test

-   전체 코드 중 가장 작은 부분을 테스트 하는 것이다.
-   함수 혹은 별개의 React 컴포넌트 코드의 한 유닛, 혹은 단위를 테스트 함.
-   유닛 테스트의 경우 테스트를 실제 서비스와 최대한 격리시킨다.
-   따라서 함수나 컴포넌트를 테스트할 때, 의존성을 반드시 표기하며 테스트 버전을 사용한다.
-   해당 유닛이 다른 코드의 유닛과 상호작용 하는 것을 테스트 하진 않는다.
-   만약 컴포넌트 관련 테스트 코드를 작성할 때, 데이터 베이스나 비동기 API 같은 외부 리소스가 포함될 경우 더 이상 유닛 테스트라고 볼 수 없음.
-   기본적으로 테스트를 위한 입력 값을 주고, 그에 대한 함수의 출력 값이 예상과 일치하는지를 판단하는 것이 유닛 테스트이다.

2. Integration Test

-   여러 유닛이 함께 작동하는 방식을 테스트 해서, 유닛 간의 상호 작용을 테스트
-   컴포넌트 간의 상호 작용, 혹은 마이크로 서비스 간의 상호 작용을 테스트 함.

3. Functional Test

-   소프트웨어의 특정 기능 혹은 동작을 테스트 하는 것.
-   예를 들어 데이터를 폼에 입력하고 제출을 클릭하면, 해당 데이터가 정상적으로 전송되는지를 체크하는 것.
-   여러 유닛이 있는 통합 테스트일 수도 있지만, 단일 유닛의 기능 테스트일 수 있음.
-   기능 테스트의 목적은 작성된 코드의 적합성을 판단하는 것이 아닌, 동작을 테스트 하는 것.
-   기능 테스트의 경우 테스트에 유저 플로우와 연관된 모든 단위를 포함시킨다.
-   프로그램에 특정한 입력을 주고, 그에 해당하는 출력을 조사하여 원하는 결과값이 나왔는지를 테스트한다.
-   따라서 사용자가 소프트웨어와 상호 작용하는 방식과 대단히 밀접한 관계를 맺게 된다.

4. Acceptance / E2E (End to End) Test

-   사용자 중심으로 처음부터 끝까지 애플리케이션의 흐름을 테스트하는 방식이다.
-   실제 브라우저가 필요하며, 애플리케이션이 연결된 서버 또한 필요하다.
-   주로 Cypress, Selenium 같은 별도의 도구로 E2E 테스트를 진행한다.

# ✒️ Unit Test vs Functional Test

#### Unit Test의 장/단점

1. 장점

-   실패 원인을 명확하게 알 수 있다. 다른 유닛과 격리된 상태에서 독립적으로 테스트를 진행하기 때문.
-   기능 테스트에 비해 훨씬 짧은 시간 내에 테스트를 마칠 수 있으며, 문제점을 발견할 가능성이 높아진다.
-   리팩토링 후에도 해당 유닛이 의도대로 작동하고 있음을 단위 테스트를 통해 파악할 수 있다.
-   각 유닛에 대한 독립적인 테스트 코드를 작성하기에 테스트 간 낮은 결합도를 보장한다.

2. 단점

-   사용자가 소프트웨어와 상호 작용하는 과정에서 실패를 하더라도 해당 테스트는 통과될 수 있음.
-   따라서 사용자와 소프트웨어와 상호 작용하는 과정보다는, 각 유닛에 대한 테스트에 중점을 둔다.
-   리팩토링으로 인해 실패할 가능성이 있음. 로직이 변경될 경우 동작이 변하지 않더라도 테스트는 실패할 수 있기 때문.

#### Functional Test의 장/단점

1. 장점

-   사용자가 소프트웨어와 상호 작용 하는 관계와 밀접하기에, 테스트가 매우 견고하다.
-   사용자가 테스트에 통과했다면 문제가 없고, 테스트에 실패하면 문제가 발생했을 가능성이 높다는 의미이기 때문.
-   코드 작성 방식을 리팩터링 할 경우에도 동작이 동일하게 유지되는 경우 테스트를 통과할 수 있음.

2. 단점

-   테스트가 독립적이지 않고 여러 요소와 결합된 형태이기에, 실패 시 어디서 오류가 났는지 찾기 어려움.
-   하지만 RTL의 경우 기능 테스트의 장점이 단점을 상쇄시킨다고 보고 있다.

# ✒️ BDD (Behavior Driven Development)

#### BDD (행동 주도 개발) 는 무엇인가?

-   TDD와는 다르게, 사용자의 행위까지 고려하여 테스트를 진행하고 개발하는 프로세스를 의미한다.
-   따라서 BDD는 시나리오를 기반으로 테스트 케이스를 작성하며, 함수 단위의 테스트를 권장하지 않는다.
-   하나의 시나리오는 Given, When, Then 구조를 가지는 것으로 기본 패턴을 권장함.

```
- Feature: 테스트 대상의 기능 및 책임을 명시.
- Scenario: 테스트 목적에 대한 상황을 설명한다.
- Given: 시나리오 진행에 필요한 상황을 설명한다.
- When: 시나리오를 진행하는데 필요한 조건을 명시한다.
- Then: 시나리오를 완료할 경우 보장해야 하는 결과를 명시한다.
```

#### BDD 방식을 모의로 적용해보자.

> 단순한 카운터 애플리케이션을 개발한다고 가정하고, BDD 패턴을 도입해보자.

-   사용자가 `+` 버튼을 눌렀을 경우

```
- GIVEN: 현재 숫자가 100 이하일 경우
- WHEN : 사용자가 `+` 버튼을 눌렀다면
- THEN: 숫자가 1 늘어난다.
```

-   사용자가 `-` 버튼을 눌렀을 경우

```
- GIVEN: 현재 숫자가 1 이상일 경우
- WHEN : 사용자가 `-` 버튼을 눌렀다면
- THEN: 숫자가 1 감소한다.
```

-   예시에서는 THEN이 1개만 기술되었으나 일반적인 프로그래밍에서는 THEN은 여러 개가 존재한다.
-   진척도를 체크할 수 있는 단위는 보통 THEN을 기준으로 하기 때문이다.
-   단, THEN에 할당된 Task의 크기가 들쭉날쭉하다면 안되므로 각 Task 별로 필요한 개발 비용을 비슷하게 쪼개야 한다.
