---
title: ":coffee: Java - @deprecated"
categories: TIL
tags: [ TIL ]
---

## `@deprecated`란?

Java API에서 `@deprecated`을 종종 찾아볼 수 있다. deprecated는 '중요도가 떨어져 더 이상 사용되지 않고 앞으로는 사라지게 되는'이라는 뜻이다. 즉 클래스나 메서드에 deprecated가 붙어 있다면 **차후에 없어질 수 있으니 사용을 권장하지 않는** 클래스나 메서드라는 뜻이다. Java API는 **하위 호환성**을 고려해 설계되었기 때문에 버전업이 되어 더 이상 사용되지 않는 클래스나 메서드라도 바로 삭제하지 않고 deprecated를 사용하여 표시한다. 지금 당장은 **기능을 하지만** 앞으로 어떻게 변할 지 모르기 때문에 가급적이면 대체되는 메서드를 찾아 사용하자.



## 예시

![deprecated](https://user-images.githubusercontent.com/50407047/89774623-319dbe00-db41-11ea-9a6b-d1c116ada27c.png)

java.util.Date의 해당 생성자는 사용을 권하지 않는 생성자이다. Java API는 Javadoc을 통해 이 사실을 알려주기 위해 아래와 같이 메서드 코드 위에 annotation 주석으로 deprecated를 표시하였다.

```java
@Deprecated
public Date(int year, int month, int date) {
    this(year, month, date, 0, 0, 0);
}
```

