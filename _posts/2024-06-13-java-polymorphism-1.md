---
title: "[JAVA] 다형성에 대해 알아보자 - 다형적 참조와 메서드 오버라이딩☕"
date: 2024-06-13
categories: [Java]
render_with_liquid: false
tags:
    [
        Java
    ]
---

![자바 다형성 썸네일](/assets/img/posts/2024-06-12-thumnail.png)

김영한의 실전 자바 중급 2편을 수강하면서 List 컬렉션에 대해 살펴보려던 중, List가 다형성과 OCP 원칙을 잘 적용한 예라는 설명을 듣게 되었다. 다형성의 개념에 대해 막연하게만 알고 있던 차라 List의 장점이나 설명이 와닿지 않을 것 같아서 이 참에 먼저 다형성에 대해 정리해보기로 했다.

# 객체 지향 프로그래밍과 다형성

객체 지향 프로그래밍의 특징에는 추상화, 캡슐화, 상속, 다형성이 있다. 그 중에서도 객체지향 프로그래밍의 꽃이라고 불리는 다형성은 하나의 메소드나 클래스가 있을 때 이것들이 다양한 방법으로 동작하는 것을 의미한다. 예를 들어 붕어빵을 만든다고 생각해보자.

![붕어빵](/assets/img/posts/2024-06-12-1.png)

따끈따끈 겨울 간식 붕어빵에는 여러 종류가 있다. 모두 같은 밀가루 반죽을 사용하지만 넣는 재료가 팥이라면 팥 붕어빵, 슈크림이라면 슈크림 붕어빵이 된다. 겉으로 보기에 같은 붕어빵이라도 넣는 재료에 따라 맛도, 식감도 달라지게 된다. 하나의 붕어빵 틀로 여러 가지 재료를 넣어 다양한 붕어빵을 만들어 낼 수 있다.

키보드의 경우도 마찬가지다. '누른다' 라는 행위는 모든 키에 똑같이 적용되지만, 각 키가 수행하는 기능은 모두 다르다.

이렇게 한 객체가 여러 타입의 객체로 취급될 수 있는 것이 바로 다형성이다.

이런 특징이 객체 지향 프로그래밍에서 어떻게 사용될까?

대규모 소프트웨어를 개발할 때 한 기능을 고치기 위해 모든 기능을 수정해야 한다면 비용와 시간이 많이 소모될 것이다. 따라서 마치 레고를 조립하듯 필요한 부분만 갈아 끼울 수 있도록 설계하는 것이 바람직하다. 이를 위해 객체 지향 프로그래밍이 도입되었다. 각각의 객체가 독립적으로 수행되고 메시지를 주고받으며 데이터를 처리함으로써, 기능을 추가하거나 수정하는 경우 해당하는 객체만 교체하도록 프로그램을 설계할 수 있다. 따라서 **프로그램을 유연하고 변경이 용이하게 만들 수 있게 된다**. 

좀 더 자세히 알아보기 위해 **다형적 참조**와 **메서드 오버라이딩**을 살펴보자.

## 다형적 참조

> 💡 자바에서 **부모 타입은 자신은 물론이고, 자신을 기준으로 모든 자식 타입을 참조할 수 있다**. 이것이 바로 다양한 형태를 참조할 수 있다고 해서 다형적 참조라고 한다. 이처럼 **부모는 자식을 품을 수 있다는 것**이 다형성의 핵심이다.

```java
Parent poly = new Parent();
Parent poly = new Child();
Parent poly = new Grandson(); // Child 하위에 손자가 있는 경우
```

그러나 주의해야 할 점은 부모 타입이 자식의 기능(메서드)을 사용할 수는 없다는 점이다.

```java
Parent poly = new Child();
poly.ChildMethod(); // 컴파일 에러
```

만약 `Child`가 `Parent`를 상속받고 있는 경우 부모 타입이 자식을 참조할 수는 있지만 `poly.ChildMethod()` 는 타입 불일치 에러를 반환한다.

왜냐하면, `poly.ChildMethod()`를 실행했을 때 먼저 참조값을 통해 인스턴스를 찾고 다음으로 인스턴스 안에서 실행할 타입을 찾아야 한다. 호출자인 `poly`는 `Parent`타입이기 때문에 `Parent` 클래스부터 시작해서 필요한 기능을 찾는다. 상속 관계는 부모 방향으로 찾아 올라갈 수는 있지만 자식 방향으로 찾아 내려갈 수는 없다. `Parent`는 부모 타입이고 상위에 부모가 없기 떄문에 `childmMethod()`를 찾을 수 없게 되는 것이다.

### 다운 캐스팅

만약 `Parent`가 `Child`의 기능을 사용하고 싶다면 **다운캐스팅**을 통해 참조 대상을 `Child` 타입으로 변경해서 사용할 수 있다. 실행 순서는 다음과 같다.

```java
Parent poly = new Parent();
Child child = (Child) poly; // 다운 캐스팅을 통해 부모타입을 자식 타입을 변환한 다음 대입 시도
Child child = (Child) x001; // 참조값을 읽은 다음 자식 타입으로 지정
Child child = x001 // 최종 결과
```

이 때 `Parent poly`의 타입이 변하는 것이 아니라 해당 참조값을 꺼내고 꺼낸 참조값이 `Child`이 되는 것이기 때문에 `poly`의 타입은 변하지 않는다.

#### ❔ 왜 다운 캐스팅은 명시적으로 선언해야 할까?

업캐스팅은 개발자가 명시하지 않아도 자바가 캐스팅 해주는 반면, 다운캐스팅의 경우는 개발자가 명시를 해주어야 한다. **다운캐스팅은 심각한 런타임 오류를 야기할 수 있기 때문**이다.

```java
// 다운캐스팅을 자동으로 하지 않는 이유
public class CastingMain4 {
    public static void main(String[] args) {
        Parent parent1 = new Child();
        Child child1 = (Child) parent1;
        child1.childMethod(); // 문제 없음

        Parent parent2 = new Parent();
        Child child2 = (Child) parent2; // 런타임 오류 - ClassCastException
        child2.childMethod(); // 실행 불가
    }
}
```

`parent1`의 경우 `Child` 클래스를 참조해 생성하고 있다. 객체를 생성할 때 해당 타입의 상위 부모 타입은 모두 함께 생성되므로 메모리에는 `Parent, Child` 타입이 모두 존재하게 된다.

`parent2`의 경우 `Parent` 클래스를 참조해 생성하고 있다. 따라서 메모리에는 `Parent` 클래스만 존재한다.

그런데 `child2`에 `Child` 클래스로 다운캐스팅한 `parent2`를 담으려 하고 있다. 자바는 메모리에서 `Child` 클래스를 찾아보지만 존재하지 않으므로 `ClassCastException` 런타임 오류를 발생시킨다.

```java
parent1 instanceof Child // 부모는 자식을 담을 수 있기 때문에 true
parent2 instanceof Child // 자식은 부모를 담을 수 없기 때문에 false
```

즉, **인스턴스에 존재하지 않은 하위 타입으로 캐스팅하는 문제가 발생할 수 있기 때문**에 개발자가 이런 문제를 인지하고 사용해야 한다는 의미로 명시적으로 캐스팅을 해주어야 한다.

## 메서드 오버라이딩

> 💡 메서드 오버라이딩은 기존 기능을 덮어 새로운 기능을 재정의 한다. 꼭 기억해야 할 점은 **오버라이딩 된 메서드가 항상 우선권을 가진다**는 점이다.

다음과 같은 `Parent, Child` 클래스를 만들고 메서드 오버라이딩에 대해 살펴보겠다.

```java
public class Parent {
    public String value = "parent";

    public void method() {
        System.out.println("Parent.method");
    }
}
```

```java
public class Child extends Parent {
    public String value = "child";

    @Override
    public void method() {
        System.out.println("Child.method");
    }
}
```

`Child`에서 `Parent`의 `method()`를 재정의했다.

```java
public class OverridingMain {
    public static void main(String[] args) {
        // 자식 변수가 자식 인스턴스 참조
        Child child = new Child();
        System.out.println("Child -> Child");
        System.out.println("value = " + child.value);
        child.method();

        // 부모 변수가 부모 인스턴스 참조
        Parent parent = new Parent();
        System.out.println("Parent -> Parent");
        System.out.println("value = " + parent.value);
        parent.method();

        // 부모 변수가 자식 인스턴스 참조(다형적 참조)
        Parent poly = new Child();
        System.out.println("Parent -> Child");
        System.out.println("value = " + poly.value); // 변수는 오버라이딩 X
        poly.method(); // 메서드 오버라이딩

    }
}
```

자식 변수가 자식 인스턴스를 참조하거나, 부모 변수가 부모 인스턴스를 참조하는 경우 당연하게도 각각 클래스에 존재하는 메서드가 실행된다.

그런데 세 번째 경우를 살펴보자. 부모 타입 변수인 `Parent poly`가 자식 인스턴스를 참고하고 있다. 이 경우 변수는 원래 타입 `Parent`의 변수 `value = parent`를 출력하지만, 메서드는 자식 클래스인 `Child`에서 재정의된 메서드가 실행되어 `Child.method`가 출력된다.

이처럼 부모 변수가 자식 인스턴스를 참조하는 다형적 참조를 하게 되면, 먼저 현재 타입에 존재하는 메서드를 찾고 재정의된 메서드가 있는지 검사한 후 있으면 재정의된 메서드를 실행한다. 없으면 가지고 있는 메서드를 사용한다.

이렇게 다형성을 구현하는 데에 중요한 개념인 다형적 참조와 메서드 오버라이딩에 대해 정리해보았다. 다음 포스팅으로는 위 개념들이 다형성을 구현하는데 왜 필요한지, 어떻게 활용될 수 있는지에 대해 알아보자.

# 참고자료
🔗 [김영한의 실전 자바 기본편](https://www.inflearn.com/course/%EA%B9%80%EC%98%81%ED%95%9C%EC%9D%98-%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94-%EA%B8%B0%EB%B3%B8%ED%8E%B8/dashboard)