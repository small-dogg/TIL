# 제네릭(Generic)

제네릭은 Java 5부터 새롭게 추가되었다. 제네릭 타입을 사용하면 잘못된 타입이 사용될 수도 있는 문제를 컴파일 단계에서 해소시킬 수 있다. 주로 컬렉션, 람다식, 스트림, NIO에서 많이 사용한다.

제네릭은 데이터의 타입을 일반회한다는 것을 의미한다. 좀더 부가적으로 설명하면 데이터 형식에 의존하지 않고, 하나의 값이 여러 다른 데이터 타입을 가질 수 있도록하는 방법으로 이해하면 된다.

일반적으로 컬렉션 프레임워크를 사용할 때를 보면

```java
List<String> stringList = new ArrayList<String>();
```

과 같은 형태로 선언을 한다.

우리는 `<>` 사이에 데이터 타입을 작성하여, 이 ArrayList에 들어가는 요소들의 타입을 사전 정의한다. 제네릭은 클래스 내부에서 타입을 의사결정하지 않고, 외부에서 클래스를 사용할 때 타입을 결정할 수 있다. 한마디로 특정 타입을 미리 지정해주는 것이 아니라 필요에 의해 지정할 수 있도록 하는 일반 타입이라는 것이다.

```java
List<String> stringList = new ArrayList<>();
```

다이아몬드 연산`<>` 을 사용하면 변수 선언 및 객체 생성 두군데 모두 타입을 지정하지 않아도 된다. 

자바 7버전 이후부터 중복으로 작성되는 타입을 한번만 명시해도 되도록 바뀌었다.

## 제네릭 장점

- 제네릭을 사용하면 잘못된 타입이 들어올 수 있는 것을 컴파일 단계에서 방지할 수 있다.
- 클래스 외부에서 타입을 지정하기 때문에 따로 타입을 체크하고 변환해줄 필요가 없다.
- 비슷한 기능을 지원하는 경우 코드의 재사용성이 높아진다.

## 제네릭 약속

일반적으로 제네릭은 아래의 타입을 사용하는 것을 암묵적으로 약속했다.

| 타입  | 설명 |
|-----| --- |
| <T&#62; | Type |
| <E&#62; | Element |
| <K&#62; | Key |
| <V&#62; | Value |
| <N&#62; | Number |

## 제네릭 타입

제네릭 타입은 타입을 파라미터로 가지는 클래스와 인터페이스를 말한다.

```java
public class ClassName <T> { ... }
public interface InterfaceName <T> { ... }
```

기본적으로 class나 interface의 경우 위와같이 제네릭 타입을 선언한다.

T 타입은 {…} 블럭 안에서까지 유효하다.

```java
ClassName obj = new ClassName<String>();
```

제네릭이 선언되어있어 클래스의 인스턴스를 만들 때, 외부에서 데이터 타입을 지정할 수 있다.

참고로, 타입 파라미터로 명시할 수 있는 것은 참조 타입 뿐이다.

## 멀티 타입 파라미터

제네릭 타입은 두개 이상 멀티 타입 파라미터를 사용할 수도 있다. HashMap을 떠올려보자.

여러개의 타입 파라미터를 지정하는 것은 단순히 `,` 로 구분하여 여러개를 나열하면 된다.

```java
public class ClassName <T, E> { ... }
public interface IterfaceName <T, E> { ... }
```

## 제네릭 메서드(<T,R> R method(T t))

제네릭 메서드는 매개 타입과 리턴 타입으로 타입 파라미터를 갖는 메서드를 말한다.

선언 방법은 리턴타입 앞에 <> 기호를 추가하고 타입 파라미터를 작성한 다음, 리턴 타입과 매개 타입으로 타입 파라미터를 사용하면 된다.

```java
public <타입파라미터, ...> 리턴타입 메서드명(매개변수, ...) { ... }

public <T> Box<T> boxing(T t){ ...}
```

제네릭 메서드는 두가지 방식으로 호출할 수 있다. 코드에서 타입 파라미터의 타입을 명시하거나, 컴파일러가 매개값의 타입을 보고 구체적인 타입을 추정할 수 있도록 할 수도 있다.

```java
Box<Integer> box = <Integer>boxing(100);
Box<Integer> box = boxing(100);
```

## 제한된 타입 파라미터(<T extends 최상위타입>)

타입 파라미터를 제한적으로 사용해야하는 경우가 종종 존재한다. 이를테면, 숫자타입만 받게한다던지 하는 경우이다.

이럴때는 타입 파라미터 뒤에 extends 키워드를 작성하고 상위 타입을 명시하면 된다. 상위타입은 클래스 뿐만아니라 인터페이스도 작성이 가능하다.

```java
public <T extends 상위타입> 리턴타입 메서드(매개변수, ...) {...}
```

## 와일드 카드 타입(<?>, <? extends …>, <? super …>)

`?` 는 일반적으로 와일드 카드라고 부른다. 제네릭 타입을 매개값이나 리턴 타입으로 사용할 때 구체적인 타입 대신에 와일드 카드를 다음과같이 세가지 형태로 사용할 수 있다.

- 제네릭타입<?> : 제한없음

타입 파라미터를 대치하는 구체적인 타입으로 모든 클래스나 인터페이스 타입이 올 수 있다.

- 제네릭<? extends 상위타입> : 상위 클래스 제한

타입 파라미터를 대치하느 구체적인 타입으로 상위 타입이나 하위타입만 올 수 있다.

- 제네릭<? super 하위타입> : 하위 클래스 제한

타입 파라미터를 대치하느 구체적인 타입으로 하위 타입이나 상위타입이 올 수 있다.

![Untitled](제네릭(Generic)/Untitled.png)

위 그림과 같은 상속관계를 가지고 있을 때,

Alphabet<?> : A부터 D까지 모두 될 수 있다.

Alphabet<? extends C> : C와 D만 될 수 있다.

Alphabet<? super B> : A와 B만 될 수 있다.

## 제네릭 타입의 상속과 구현

제네릭 타입도 다른 타입과 마찬가지로 부모 클래스가 될 수 있다.

```java
public class ChildProduct<T,M> extends Product<T, M> {...}
```

자식 제네릭 타입은 추가적인 타입 파라미터를 받을 수 있다.

```java
public class ChildProduct<T, M, C> extends Product<T, M> {...}
```

인터페이스도 마찬가지로 구현할 수 있고, 타입 파라미터를 추가적으로 받을 수 있다.

```java
public class Car<K,V> implements Vehicle<K> {...}
```