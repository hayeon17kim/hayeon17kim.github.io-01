---
title: ":book: 리팩토링 1장 (2): Replace Conditional with Plymorphism"
categories: book
tags: [ java ]
---

# 리팩토링

![리팩토링](https://bookthumb-phinf.pstatic.net/cover/070/476/07047630.jpg?type=m5)

마틴 파울러의 "리팩토링: 코드 품질을 개선하는 객체지향 사고법"을 읽으면서 공부한 내용을 올립니다. 

첫 장은 주어진 프로그램 코드를 리팩토링하면서 전반적인 리팩토링의 유용성과 방법을 조망하는 챕터입니다.



## 가격 책정 부분의 조건문을 재정의로 교체

- **조건문의 인자나 조건식의 변수**는 타 객체 데이터를 사용하지 말고 **자신의 데이터를 사용**하는 것이 좋다.
- Rental 클래스의 `getCharge()`, `getFrequentRenterPoints()`  메서드는 다음과 같이 타 객체(`Movie`)의 데이터를 사용하고 있다.

```java
// Rental 클래스

  double getCharge() {
    double result = 0;
      // switch문의 인자로 타 객체의 데이터를 사용
    switch (getMovie().getPriceCode()) {
      case Movie.REGULAR:
        result += 2;
        if (getDaysRented() > 2)
          result += (getDaysRented() - 2) * 1.5;
        break;
      case Movie.NEW_RELEASE:
        result += getDaysRented() * 3;
        break;
      case Movie.CHILDRENS:
        result += 1.5;
        if (getDaysRented() > 3)
          result += (getDaysRented() - 3) * 1.5;
        break;
    }
    return result;
  }

  int getFrequentRenterPoints() {
      // if문의 조건식에 타 객체의 데이터 사용
    if ((getMovie().getPriceCode() == Movie.NEW_RELEASE) && getDaysRented() > 1)
      return 2;
    else
      return 1;
  }
```

- 따라서 데이터를 사용하는 클래스(Movie)에 메서드를 만들고, Rental 클래스의 메서드는 Movie의 메서드를 호출하도록 만들었다.

```java
// Movie 클래스

  // 대여일은 매개변수로 전달한다.
  double getCharge(int daysRented) {
    double result = 0;
    // 이제 소속 클래스의 데이터를 인자로 사용할 수 있다.
    switch (getPriceCode()) {
      case Movie.REGULAR:
        result += 2;
        if (daysRented > 2)
          result += (daysRented - 2) * 1.5;
        break;
      case Movie.NEW_RELEASE:
        result += daysRented * 3;
        break;
      case Movie.CHILDRENS:
        result += 1.5;
        if (daysRented > 3)
          result += (daysRented - 3) * 1.5;
        break;
    }
    return result;
  }

  int getFrequentRenterPoints(int daysRented) {
    // 이제 조건식에서 소속 클레스의 데이터를 사용할 수 있다.
    if ((getPriceCode() == Movie.NEW_RELEASE) && daysRented > 1)
      return 2;
    else
      return 1;
  }
```

```java
// Rental 클래스

  double getCharge() {
    return _movie.getCharge(_daysRented);
  }

  int getFrequentRenterPoints() {
    return _movie.getFrequentRenterPoints(_daysRented);
  }
```



## 상속 구조 만들기

- 현재 Movie 클래스는 비디오 종류에 따라 같은 메서드 호출에도 각기 다른 값을 반환하는데, 이는 하위클래스가 처리할 일이다. 따라서 final 변수가 아니라 Movie 클래스를 상속받는 3가지(RegualrMovie, ChildrensMovie, NewReleaseMovie) 하위클래스를 작성하고, 비디오 종류별 대여료 계산을 각 하위 클래스에 넣는다.



### 분류 부호를 상태/전략 패턴으로 전환

#### 전략 패턴 (Strategy Pattern)이란?

- 클라이언트 객체가 컨텍스트 객체에게 다른 특정 객체를 지정해주고 실행하게 한다. 프로그램 실행 시 컨텍스트 객체가 실행할 객체를 외부(클라이언트 객체)에서 유연하게 지정할 수 있다는 것이 장점이다. 컨텍스트 객체 내에서 참조 멤버변수를 사용하여 하위 클래스의 객체의 참조를 전달하면 컨텍스트 객체의 코드 변경 없이도 컨텍스트의 행동을 변경할 수 있다. 즉 다형성과 가상 메서드 호출을 이용하여 컨텍스트 클래스의 코드를 간략하게 할 수 있다.

#### 상태 패턴 (State Pattern)이란?

- 가상 메서드 호출을 이용한다는 점에서 Strategy Pattern과 유사하다.
- State Pattern은 외부(클라이언트)의 개입 없이 상태객체 내부에서 현재 상태에 따라 컨텍스트 객체의 멤버인 상태객체를 변경하여 사용한다는 점에서 차이가 있다. 따라서 프로그램의 각 상태를 객체화하여 사용한다는 점이 요구된다. 

> 인다이렉션(indirection): 값 자체가 아니라 이름, 참조, 컨테이너 등을 사용하여 대상을 참조하는 기능

- 분류 부호에 필드 자체 캡슐화 기법 적용

```java
  public Movie(String title, int priceCode) {
    _title = title;
    setPriceCode(priceCode); //쓰기 메서드
  }
```

- Price 클래스를 상속 확장하는 클래스 작성
- Price 클래스에는 **추상 메서드**를 넣어 **종류 판단** 기능 제공

```java
abstract class Price {
  abstract int getPriceCode();
}


class ChildrensPrice extends Price {
  int getPriceCode() {
    return Movie.CHILDRENS;
  }
}


class NewReleasePrice extends Price {
  int getPriceCode() {
    return Movie.NEW_RELEASE;
  }
}


class RegularPrice extends Price {
  int getPriceCode() {
    return Movie.REGULAR;
  }
}

```

- 분류 부호의 기능을 상태 패턴 안으로 옮긴다.
- 메서드 이동 기법으로 switch문을 Price 클래스 안으로 옮긴다.
- 조건문을 재정의로 전환 기법으로 swtich 문을 없앤다.

```java
package com.monica.rms.Exam08;

abstract class Price {

  abstract double getCharge(int daysRented);

  abstract int getPriceCode();

  int getFrequentRenterPoints(int daysRented) {
    return 1;
  }
}


class ChildrensPrice extends Price {

  double getCharge(int daysRented) {
    double result = 2;
    if (daysRented > 2)
      result += (daysRented - 2) * 1.5;
    return result;
  }

  int getPriceCode() {
    return Movie.CHILDRENS;
  }
}


class NewReleasePrice extends Price {
  double getCharge(int daysRented) {
    return daysRented * 3;
  }

  int getPriceCode() {
    return Movie.NEW_RELEASE;
  }

  int getFrequentRenterPoints(int daysRented) {
    return (daysRented > 1) ? 2 : 1;
  }

}


class RegularPrice extends Price {
  double getCharge(int daysRented) {
    double result = 1.5;
    if (daysRented > 3)
      result += (daysRented - 3) * 1.5;
    return result;
  }

  int getPriceCode() {
    return Movie.REGULAR;
  }
}

```

- 원래 코드가 있었던 Movie 메서드는 Price의 메서드를 호출하도록 만든다. 

```java
package com.monica.rms.Exam08;

// 비디오 데이터 클래스
class Movie {
  public static final int CHILDRENS = 2;
  public static final int REGULAR = 0;
  public static final int NEW_RELEASE = 1;
  private String _title;
  private int _priceCode;
  private Price _price;

  public Movie(String title, int priceCode) {
    _title = title;
    setPriceCode(priceCode);
  }

  public int getPriceCode() {
    return _priceCode;
  }

  public void setPriceCode(int arg) {
    switch (arg) {
      case REGULAR:
        _price = new RegularPrice();
        break;
      case CHILDRENS:
        _price = new ChildrensPrice();
        break;
      case NEW_RELEASE:
        _price = new NewReleasePrice();
        break;
      default:
        throw new IllegalArgumentException("가격 코드가 잘못됐습니다.");
    }
  }

  public String getTitle() {
    return _title;
  }

  double getCharge(int daysRented) {
    return _price.getCharge(daysRented);
  }

  int getFrequentRenterPoints(int daysRented) {
    return _price.getFrequentRenterPoints(daysRented);
  }

}

```



- 리팩토링 전

![image](https://user-images.githubusercontent.com/50407047/91439713-68493780-e8a8-11ea-8224-29f8f76ea493.png)

- 리팩토링 후

![image](https://user-images.githubusercontent.com/50407047/91439727-6ed7af00-e8a8-11ea-8a68-79b113436cdf.png)