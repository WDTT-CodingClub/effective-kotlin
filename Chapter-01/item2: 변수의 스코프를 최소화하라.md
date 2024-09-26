## 변수의 스코프를 최소화하라

변수와 프로퍼티의 스코프는 최대한 작게
- 프로퍼티보단 지역 변수가 좋다.
- 스코프는 최대한 좁게하라

스코프는 오직 내부에서 외부 스코프에 있는 요소로만 접근 가능함

그렇기 때문에 변수는 스코프를 좁게 써야한다. 추적하기 용이하기 때문

변수는 읽고 쓰기 전용 여부와 상관 없이, 변수를 정의할 때 초기화되는 것이 좋다.
여러 프로퍼티를 한번에 설정해야 하는 경우에는 구조분해 선언을 활용하면 좋다.

### 캡처링

```kotlin
  val primes: Sequence<Int> = sequence {
    var numbers = generateSequence(2) { it + 1 }

    var prime: Int
    while(true) {
      prime = numbers.first()
      yield(prime)
      numbers = numbers.drop(1)
        .filter { it % prime != 0 }
    }
  }
```
에레토스테네스의 체를 구현해보는 코드이다.

하지만 이렇게 구현할시 결과가 이상하게 나온다?

왜?why?
 prime이라는 변수가 while문 밖에서 캡처되었기 때문
 시퀀스는 지연평가를 하기 때문에 yield가 되고 필터링이 지연된다.
 그래서 최종적인 prime 값으로만 필터링 된 것
