# ✒️ Generator

#### 1. Generator 함수란?

- Generator 함수란, 원하는 만큼 코드 실행을 시작하거나 중단할 수 있는 함수.
- 중지된 제네레이터 함수를 재시작할 경우, 데이터를 추가로 전달할 수도 있음.
- 한번의 실행으로 끝나는 함수와 다르게, 사용자의 요구에 따라 제어가 가능함.

#### 2. Generator 란?

1. 정의

- Generator Function을 호출할 경우 반환되는 값
- iterable protocal을 준수해야 하며, iterator 객체이기도 하다.
- 즉, `iter[Symbol.iterator] === iter` 를 만족하는 Well - formed Iterable 이다.

2. yield, next

- yield는 제네레이터 함수의 실행을 일시적으로 **정지시킨다.**
- yield 뒤에 오는 표현식은 제네레이터의 caller에게 반환된다.
- next는 제네레이터 함수를 재실행한다.
