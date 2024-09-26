## 최대한 플랫폼 타입을 사용하지 말라

널 안정성은 코틀린의 주요 기능 중 하나이다.

자바에선 String타입을 리턴할때 @Nullable 애노테이션이 붙어 있다면 nullable로 추정하고 String?으로, @NotNull이 붙어 있다면 String으로 변경해서 받으면된다.

애노테이션이 없다면 Java에선 모든 것이 nullable 일 수 있으므로 안전하게 nullable로 가정하고 다루어야 한다.

null이 아니라고 확신 할땐 !!를 붙일 수 있다.

리스트를 사용할땐 리스트 자체 뿐만 아니라, 리스트 내부의 객체들 또한 null이 아니라는 것을 알아야 한다.

코틀린은 자바 등의 다른 프로그래밍 언어에서 넘어온 타입들을 특수하게 다루는데, 이러한 타입을 플랫폼ㅁ 타입이라고 한다.


플랫폼 타입은 String! 처럼 이름 뒤에 ! 기호를 붙여서 표기한다.

Java를 Kotlin과 함께 사용할 때, 자바 코드를 직접 조작할 수 있다면, 가능한 @Nullable 과 @NotNull 애노테이션을 붙여서 사용하는게 좋다.


코틀린에서도 위와 관련된 코드를 작성할 수 있지만, 플랫폼 타입은 안전하지 않으므로, 최대한 빨리 제거하는게 좋다.

```kotlin
// 자바
public class JavaClass {
	public String getValue() {
		return null;
	}
}

// 코틀린
fun statedType() {
    val value: String = JavaClass().value // NPE
    // ...
    println(value.length)
}

fun platformType() {
    val value = JavaClass().value
    // ...
    println(value.length) // NPE
}
```

- 두 가지 모두 NPE가 발생한다.
- statedType에서는 자바에서 값을 가져오는 위치에서 NPE가 발생한다. 이 위치에서 오류가 발생하면, null이 아니라고 예상을 했지만 null이 나온다는 것을 굉장히 쉽게 알 수 있다.
- platformType에서는 값을 활용할 때 NPE가 발생한다. 객체를 사용한다고 해서 NPE가 발생될 거라고 생각하지는 않으므로, 오류를 찾는데 굉장히 오랜 시간이 걸리게 될 것이다.

인터페이스에서 다음과 같이 플랫폼 타입을 사용했다고 해보자.

```kotlin
interface UserRepo {
	fun getUserName() = JavaClass().value
}
```

이러한 경우 메서드의 inferred 타입(추론된 타입)이 플랫폼 타입이다. 이는 누구나 nullalbe 여부를 지정할 수 있다는 것이다.
