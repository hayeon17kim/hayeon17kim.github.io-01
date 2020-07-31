

## Today I Learned

### 클래스

![class](https://m.jubangtong.com/web/product/big/201803/4256_shop1_851864.jpg)

- **사용자 정의 데이터타입**(User Defined Data type) 
  - 사용자 : 개발자



```c
#include <stdio.h>
#include <stdlib.h>

struct Member {
    int no;
    char name;
}

int main() {
    struct Member m1;
}
// new 없이 이렇게 바로 만듦
```



인스턴스의 주소는 JVM이 절대 알려주지 않음. 그런 명령어 없음



배열로 레퍼런스를 만들 때는 null 로 초기화된다. (자동 초기화된다.)

new 로 만드는 것은 무조건 자동 초기화. 

```java
    // Member 설계도에 따라 준비한 메모리의 주소를 담을 변수 선언 
    Member m;
    
    // Member 설계도에 따라 메모리 생성
    // 인스턴스 만드는 명령어
    m = new Member();
    
    //m 에 저장된 주소로 찾아가서 해당 인스턴스의 각 항목에 데이터 넣기 
    m.no = 1;
    m.name = "홍길동";
    m.email = "hong@test.com";
    m.password = "1111";
    m.photo = "hong.gif";
    m.tel = "010-1111-22222";
    m.createdDate = Date.valueOf("2020-1-1"); // 분석해서 날짜 데이터로 만든 후 저장
    
    //m에 저장된 주소로 찾아가서 해당인스턴스의 각 항목의 값을 꺼내기
    System.out.println(m);
    // => m이라는 변수에 저장된 주소로 찾아가서 no이라는 이름의 항목 값을 꺼내기
    // => 레퍼런스 m이 가리키는 인스턴스의 no 항목의 값을 꺼내기
    // => 레퍼런스 m이 가리키는 인스턴스의 no 필드 값꺼내기
    // => m 객체의 no 필드 값 꺼내기
    
    // m 객체: m이 가리키는 인스턴스 (m이 가리키는 객체)
    // m이 객체가 아니라 
    
    // 이제 assistance 창에 우리가 만든 필드가 나타남!
    
    
    // 인스턴스는 클래스의 변수 선언하고만 관계있음
    // 인스턴스는 메소드와 아무 상관 없음 
    
    Member m2;
    m2 = m;
    m2.name = "임꺽정";
    
    System.out.println(m2.name);
    
    //
    Member[] members = new Member[4];
    // 레퍼런스를 4개 만든 것. 인스턴스를 4개 만든 게 아니라!!!!!!!!!!!!!!
    // 인스턴스 만드는 게 아님
    
    // 인스턴스를 만들어서 각각의 주소값을 레퍼런스에 저장
    members[0] = new Member();
    members[1] = new Member();
    members[2] = new Member();
    
    members[0].name = "홍길동";
    //0번에 저장된 항목으로 찾아가서 name에 홍길동을 집어넣어라.
    members[1].name = "임꺽정";
    // 1700번지로 찾아가서 name 필드에 임꺽정을 집어넣어라 
    members[2].name = "유관순";
    // 2번 레퍼런스에 저장된 주소로 찾아가서 그 인스턴스의 name항목에 유관순을 집어넣어라.
    members[0].name = "안중";
    
    //memebers 가 가리키는 배열의 2번방에 저장되어 있는 주소로 찾아가라. 
    Member x;
    x = members[2];
    x.name = "ohora";
    x = members[0];
    x.name = "woohehe";
    
    Member x2;
    x2 = x;
    x2.name ="her";
    
    System.out.println(members[0].name);
    System.out.println(members[1].name);
    System.out.println(members[2].name);
    System.out.println(members[3].name);
```

![식판카트](https://www.jubangbank.co.kr/data/base/goods/middle/201803063271899.jpg)

식판을 만들기 전에 식판을 꽂을 레퍼런스를 준비한다. 