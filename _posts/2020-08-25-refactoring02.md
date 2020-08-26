---
title: ":book: 리팩토링 1장 (2): 조건문을 재정의로 교체"
categories: book
tags: [ java ]
toc: true
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

