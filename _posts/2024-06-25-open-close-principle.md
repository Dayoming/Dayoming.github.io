---
title: "[OOP] 객체지향 5대 원칙(SOLID) - 개방 폐쇄 원칙(Open-Closed Principle)"
date: 2024-06-25
categories: [Java]
render_with_liquid: false
tags
    [
        Java,
        OOP
    ]
---

![ocp thumbnail](/assets/img/posts/2024-06-25-ocp-thumbnail.png)

# Open-Close Principle
> 💡 확장에 대해 열려 있어야 하고, 변경에 대해서는 닫혀 있어야 한다는 원칙

좋은 객체지향 프로그래밍의 5대 원칙 중 하나인 개방-폐쇄 원칙(OCP; Open-Closed Principle)은 소프트웨어 개체는 **확장에 대해 열려 있어야 하고, 변경에 대해서는 닫혀 있어야 한다**는 원칙이다. 좀 더 자세히 설명하면 이렇다.

- Open for extension: 새로운 기능의 추가나 변경 사항이 생겼을 때, 기존 코드는 확장할 수 있어야 한다.
- Closed for modification: 기존의 코드는 수정되지 않아야 한다.

새로운 기능이 추가 될 때 확장이 가능하지만, 기존의 코드는 수정되지 않아야 한다는 거다.

엥? 솔직히 다시 읽어도 잘 모르겠다. 확장은 되는데 변경은 안된다고? 모순적인 말이지만 객체지향의 **다형성**과 **추상화**를 활용하면 마법처럼 불가능을 가능으로 만들 수 있다.

# 코드로 살펴보는 개방 폐쇄 원칙
## OCP 원칙 적용 전

지금부터 개방 폐쇄 원칙을 이해하기 위해 코드를 짜보자.

여러분은 쇼핑몰 시스템에서 상품의 할인율을 적용하는 프로그램을 만들어야 한다.

상품 클래스와 할인율을 계산해주는 클래스를 단순하게 만들어보자.

![diagram version 1](/assets/img/posts/2024-06-25-1.png)


```java
public class Product {
    String type;
    double price;

    Product(String type, double price) {
        this.type = type;
        this.price = price;
    }
}

public class DiscountCalculator {
    public double calculateDiscount(Product product) {
        if (product.type.equals("electronics")) {
            return product.price * 0.90; // 10% discount
        } else if (product.type.equals("clothing")) {
            return product.price * 0.80; // 20% discount
        } else if (product.type.equals("food")) {
            return product.price * 0.95; // 5% discount
        }
        return product.price;
    }
}

public class Main {
    public static void main(String[] args) {
        DiscountCalculator calculator = new DiscountCalculator();

        Product laptop = new Product("electronics", 1000);
        Product shirt = new Product("clothing", 50);
        Product apple = new Product("food", 2);

        System.out.println("Laptop price after discount: " + calculator.calculateDiscount(laptop));
        System.out.println("Shirt price after discount: " + calculator.calculateDiscount(shirt));
        System.out.println("Apple price after discount: " + calculator.calculateDiscount(apple));
    }
}
```

상품 타입과 가격을 가지고 있는 `Product` 클래스와 상품 타입에 따라 가격을 계산해주는 `DiscountCalculator` 클래스를 이용해 프로그램을 짰다. 원하는 기능은 모두 잘 동작하니 뿌듯하다고 생각할 무렵,

> 여러분이 만약 10개의 상품 카테고리를 더 만들어야 한다면?

`DiscountCalculator` 클래스를 하나하나 수정해주어야 하는 상황에 처하게 된다. 지금은 아주 단순한 로직이라 크게 오래걸리지 않을 것 같지만, 만약 대형 프로젝트에서 이런 일이 발생한다면 열 배는 더 복잡한 로직을 10개나 더 만들어주어야 하는 불편함이 생긴다. 이런 코드는 OCP 규칙을 위배하는 객체지향적이지 못한 코드다. 상품이 바뀌더라도 할인율을 계산하는 방식은 동일해야 한다.

그렇다면 어떻게 OCP 원칙을 적용해서 `DiscountCalculator`의 변경을 없앨 수 있을까?

## OCP 원칙 적용 후

![diagram version 2](/assets/img/posts/2024-06-25-2.png)


먼저, `Product` 클래스는 인터페이스로 만들어 상품 카테고리들이 구현하도록 만들어준다.

```java
public interface Product {
    double getPrice();
    double getDiscountedPrice();
}
```

이렇게 하면 2가지 이점을 얻을 수 있다.
1. 만들어진 상품 카테고리들이 각각의 할인율과 가격을 갖는 메서드를 구현하도록 강제할 수 있다.
2. `Product` 인터페이스를 구현하는 객체를 모두 `Product` 객체를 이용해 사용할 수 있다.

특히 두 번째 장점에 주목할 필요가 있다. 지금부터 만들 `Clothing, Food, Electronics` 같은 상품 하위 카테고리 클래스들은 모두 `Product` 클래스를 통해 가져와 사용할 수 있다.
이는 `다형성`의 특징인 **다형적 참조**를 통해 가능해진다.

```java
public class Clothing implements Product {
    double price;

    Clothing(double price) {
        this.price = price;
    }

    @Override
    public double getPrice() {
        return price;
    }

    @Override
    public double getDiscountedPrice() {
        return price * 0.80; // 20% discount
    }
}

public class Electronics implements Product {
    double price;

    Electronics(double price) {
        this.price = price;
    }

    @Override
    public double getPrice() {
        return price;
    }

    @Override
    public double getDiscountedPrice() {
        return price * 0.90; // 10% discount
    }
}

public class Food implements Product {
    double price;

    Food(double price) {
        this.price = price;
    }

    @Override
    public double getPrice() {
        return price;
    }

    @Override
    public double getDiscountedPrice() {
        return price * 0.95; // 5% discount
    }
}
```

이렇게 세 개의 카테고리를 만들었다. 이 클래스들은 `DiscountCalculator`의 `calculateDiscount()` 메서드에서 사용된다.

```java
public class DiscountCalculator {
    public double calculateDiscount(Product product) {
        return product.getDiscountedPrice();
    }
}
```

만약 다형성을 사용하지 않았더라면 `DiscountCalculator`의 코드는 다음과 같았을 것이다.

```java
public class DiscountCalculator {
    public double foodCalculateDiscount(Food food) {
        return food.getDiscountedPrice();
    }

    public double clothingCalculateDiscount(Clothing clothing) {
        return clothing.getDiscountedPrice();
    }

    public double electronicsCalculateDiscount(Electronics electronics) {
        return electronics.getDiscountedPrice();
    }
}
```

그러나 **다형성**의 특징, **"부모는 자식을 품을 수 있다"** 덕분에 `Product` 클래스를 매개변수로 가져와도 `Product`를 상속받는 모든 구현체들을 가져와 사용할 수 있는 것이다.

```java
public class Main {
    public static void main(String[] args) {
        DiscountCalculator calculator = new DiscountCalculator();

        Product laptop = new Electronics(1000);
        Product shirt = new Clothing(50);
        Product apple = new Food(2);

        System.out.println("Laptop price after discount: " + calculator.calculateDiscount(laptop));
        System.out.println("Shirt price after discount: " + calculator.calculateDiscount(shirt));
        System.out.println("Apple price after discount: " + calculator.calculateDiscount(apple));
    }
}
```

아까보다 코드가 길어져 얼핏보면 더 복잡해 보인다.
그러나 아까 가정했던 10개의 카테고리를 추가하는 상황을 상상해보자.

우리는 카테고리 클래스를 각각 만들어 주고, `Main`에 추가해주기만 하면 된다.
아까처럼 `DiscountCalculator`에 `if`문을 추가해주고 안에 로직을 넣어줄 필요가 없다. 기존의 코드가 전혀 수정되지 않는다ㅡ즉, 확장에는 열려 있고 변경에는 닫혀 있다!


# ✏️ 정리 - OCP 원칙을 적용하면 얻을 수 있는 장점

OCP를 적용한 코드와 OCP를 적용하지 않은 코드를 살펴보면서 어떤 차이점이 있는지 살펴보았다. OCP는 `다형성`과 `추상화`를 이용해 다음과 같은 장점을 얻을 수 있었다.

1. 코드의 유지보수성과 확장성이 높아진다.
2. 기존 코드를 수정하지 않아도 새로운 기능을 추가하거나 기능을 변경할 수 있다.
3. 변경에 대한 영향을 최소화하면서 기능을 추가하거나 변경할 수 있다.

지금 생각나는 또 다른 예시는 **Spring의 AOP**인데, 공통 관심 사항과 핵심 관심 사항을 분리하고 변경이 필요하면 그 로직만 변경할 수 있다는 점이 OCP가 잘 적용된 예시인 것 같다.

처음에 설계하기가 까다로울 수 있지만 기능 확장에 있어서 놀라울 만큼 큰 능력을 발휘하기 때문에 꼭 적용해야 할 원칙이다. 앞으로 나도 프로그램을 짤 때나 리팩토링 할 때 꼭 적용해보아야겠다고 생각했다😼