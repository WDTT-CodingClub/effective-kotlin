## 가변성 제한

Kotlin에선 var 또는 mutable 한 객체를 구성해 상태를 유지할 수 있다.

```kotlin
class BankAccount {
	var balance = 0.0
	private set

	fun deposit(depositAmount: Double) {
		balance += depositAmount
	}
	
	@Throws(InsufficientFunds::class)
	fun withdraw(withdrawAmount: Double) {
		if (balance < withdrawAmount) {
			throw InsufficientFunds()
		}
		balance -= withdrawAmount
	}
}

class InsufficientFunds : Exception()
val account = BankAccount()
println(account.balance) // 0.0
account.deposit(100.0)
println(account.balance) // 100.0
account.deposit(50.0)
println(account.balance) // 50.0
```
계좌를 관리하는 BankAccount라는 class가 있는데, var 변수로 구성되어 있다.

변수기 때문에 변화 시키는건 가능하지만 다음과 같은 문제가 있다.

1. 변형 지점이 많은 프로그램은 이해하고 디버깅하기 어렵다 
    - 변형지점이 많아질수록 추적하기가 힘들어짐
    - 이해하고 수정이 힘듬
    - 예기치 않은 상황이나 오류가 발생할때 더욱 문제
2. 코드 실행 추론하기 힘들어짐
    - 언제든 값이 변할 수 있기 때문에, 값을 확인했다고 해서 이후에도 동일하다는 보장이 없음
> ```kotlin
>var a : Int? = null
>
>a = 1
>println(a + 3) // 에러
>// a에 1을 넣어 줬지만 null이 아니라는 보장이 없기 때문에 컴파일시 에러가 발생함
>```

3. 멀티스레드 프로그램에서는 적절한 동기화가 필요함
    - 가변한 상태를 여러 스레드에서 동시에 사용하게되면 충돌할 위험이 있음 
4. 테스트가 힘듬
    - 가변성이 많아질 수록 테스트 해야하는 상태가 많아짐
5. 상태 변경이 일어날 때 이러한 변경을 다른 부분에 알려야 하는 경우가 있다.

## 가변성 제한하기

코틀린은 가변성을 제한할 수 있음
- 상수, 읽기 전용 프로퍼티 (val)
- 가변 컬렉션과 읽기 전용 컬렉션 구분하기 (ex: MutableList, List, MutableSet, Set..)
- 데이터 클래스의 copy

1. 상수
- 일반적인 방법으로는 값이 변하지 않음
- 완전히 변경 불가능한 건 아님, mutable 객체를 담고 있으면 내부적으로 변경 가능
```kotlin
val list = mutableListOf(1,2,3)
list.add(4)
```
var로 변경한것과 내부적으로 변경한 것의 차이는 객체의 재할당에 있음
val은 메모리상에서 변수의 참조가 고정되어 있음, 한 번 초기화된 후 새로운 객체로 재할당할 수 없지만, 객체의 내부 상태는 변경할 수 있다.
var는 변수자체가 재할당 될수있어 다른 객체로 변경이 가능함, 메모리 참조가 바뀔 수 있다.
- 읽기 전용 프로퍼티는 다른 프로퍼티를 활용하는 사용자 정의 게터로도 정의가 가능함.
var프로퍼티를 사용하는 val프로퍼티는 var프로퍼티가 변할 때 변할 수 있음.
```kotlin

2. ㅇ
3. 
