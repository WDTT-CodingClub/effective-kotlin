## inferred 타입으로 리턴하지 말라

코틀린의 타입 추론(type inference)은 JVM 세계에서 가장 널리 알려진 코틀린의 특징이다.

자바도 자바10부터 코틀린을 따라 타입 추론을 도입했다.

하지만 타입 추론을 사용할 때 몇가지 위험한 부분들이 있다.
이러한 위험한 부분을 피하려면 inferred타입은 오른쪽에 있는 피연산자에 맞게 설정된다는 것을 기억해야 한다.

```kotlin
  open class Animal
  class Zebra: Animal()

  fun main() {
	  var animal = Zebra()
	  animal = Animal() // 오류: Type mismatch

  fun main() {
	  var animal: Animal = Zebra()
	  animal = Animal() // 성공
```

일반적인 경우에는, 그냥 원하는 타입보다 제한된 타입이 설정되었다면, 타입을 명시적으로 지정해서 이러한 문제를 해결 할 수 있다.

하지만 직접 라이브러리(도는 모듈)를 조작할 수 없는 경우에는 이러한 문제를 간단하게 해결할 수 없다.

그리고 이러한 경우에서 inferred타입을 노출하면 위험한 일이 발생할 수 있다.

```kotlin
interface CarFactory {
	fun produce(): Car
}

val DEFAULT_CAR: Car = Fiat126P()
```

코드를 작성하다 보니 DEFAULT_CAR는 Car로 명시적으로 지정되어 있으므로 따로 필요 없다고 판단해서, 함수의 리턴 타입을 제거했다고 해보자.

```kotlin
interface CarFactory {
	fun produce() = DEFAULT_CAR
}

val DEFAULT_CAR = Fiat126()
```

CarFactory는 이제 Fiat126P 이외의 자동차를 생산하지 못한다.

리턴 타입은 API를 잘 모르는 사람에게 전달해 줄 수 있는 중요한 정보이다. 따라서 타입은 외부에서 확인할 수 있게 명시적으로 지정해 주는 것이 좋다.
