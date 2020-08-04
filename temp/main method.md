// 파라미터의 타입이 String[] 타입이어야 함. 
// 자기가 요구하는 entry 포인트가 없다고 에러가 뜨는 것

public static void main(String args) 

이클립스에서 하면 값을 넘길 수 없음.

```java
  public static void main(String[] args) throws Exception {
    System.out.println(args.length);
    for (int i = 0; i < args.length; i++) {
      System.out.println(args[i]);
    }
  }
```

```zsh
(master)⚡ % java -cp bin/main com.eomcs.basic.ex07.Exam0510 aa bb cc 
3
aa
bb
cc

```

공백으로 잘라서

args의 용도: 바깥에서 명령창으로 실행하여서 값을 넘겨줄 때

클래스 뒤에 두는 건 프로그램 argument 

클래스 앞에 두는 건 JVM argument 



m