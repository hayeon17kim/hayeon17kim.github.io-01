---
title: "🍵 부동소수점의 쓰레기값 처리하기"
categories: java
tags: [ java, linux ]
toc: true
---

## 0. 부동소수점의 연산

실제로 부동소수점(실수) 연산을 다루는 것은 쉽지만, 컴퓨터의 부동소수점 연산 처리는 쉽지만은 않다. 컴퓨터에서 값은 2진수로 다루고, 이 때문에 컴퓨터가 부동소수점 값을 연산할 때 IEEE 754 명세에 따라 작업을 수행한다. 이 과정에 값의 왜곡이 발생할 수 있다. 이 문제는 CPU나 OS, JVM의 문제가 아니라 명세에 따라 부동소수점을 처리하는 모든 컴퓨터에서 발생하는 문제이다. 




## 1. 더하기 연산에서 부동소수점 극소수값 처리
```java
double d1 = 987.6543;
double d2 = 1.111111;
System.out.println(d1 + d2 == 988.765411);	// false
System.out.println(d1 + d2);				// 988.7654110000001
// 결과 뒤에 극소수의 값이 붙는다.

double x = 234.765411;
double y = 754.0;
System.out.println(x + y == 988.765411);	// true
System.out.println(x + y);					// 988.765411
// 결과 뒤에 극소수의 값이 붙지 않았다.

System.out.println(d1 + d2 == x + y); 		// false

//---------------------------------------------------------------------
double EPSILON = 0.00001;
System.out.println(Math.abs(d1 + d2 - (x + y)) < EPSILON);		//true
```

위의 코드에서 d1 + d2에는 극소수의 값이 붙고,  x + y에는 뒤에 극소수의 값이 붙지 않았다. 따라서 같은 값이 나와야 함에도 불구하고 극소수의 값 때문에 비교연산자를 사용했을 때 false가 나오게 되었다.  부동소수점의 값을 비교할 때 IEEE 754 변환 공식에 따라 발생하는 이러한 문제를 해결하기 위해서는 극소수의 값을 자르는 코딩을 해주어야 한다.

1. EPSILON  값을 선언한다. 
2. 각각 연산에서 계산한 값의 오차를 구한다.
3. `Math.abs()` 메소드를 사용하여 오차에 절대값을 구한다. 
4. 이 오차의 절대값이 EPSILON보다 작다면 두 결과값을 같은 값으로 취급한다.



### 2. 곱하기 연산에서 부동소수점 극소수값 처리

```java
float f1 = 0.1f;
float f2 = 0.1f;

System.out.println(f1 * f2 == 0.01f); // false
System.out.println(f1 * f2); // 0.010000001

// 해결책?
// => 오차를 제거한 후 비교
// => 다만 계산 결과를 절대값으로 만든 후에 오차 이하의 값인지 비교하라!
float r = f1 * f2 - 0.01f;
System.out.println(Math.abs(r) <= Float.POSITIVE_INFINITY);
```



### 3. 





Gradle로 생성한 프로젝트 설정 파일과 폴더를 확인하자.

```
├── build.gradle
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar <== (1)
│       └── gradle-wrapper.properties
├── gradlew <== (2)
├── gradlew.bat
├── settings.gradle <== (3)
└── src
    ├── main
    │   ├── java
    │   │   └── com
    │   │       └── eomcs
    │   │           └── pms
    │   │               └── App.java
    │   └── resources
    └── test
        ├── java
        │   └── com
        │       └── eomcs
        │           └── pms
        │               └── AppTest.java
        └── resources

```
- (2)`gradlew`: gradle로 빌드한 프로젝트를 누군가 서버에 올리고 동료가 클론했을 때, 동료는 굳이 따로 gradle을 설치하지 않아도 `gradlew` 파일이 (1) `gradle-wrapper.jar`를 풀어서 실행할 수 있게 한다.
- (3)`settings.gradle`: 프로젝트 이름은 `settings.gradle` 파일 안에 `rootProject.name = '프로젝트이름'`으로 적혀 있다.

## 2. 자바 애플리케이션 실행하기

`gradle run`

- gradle 도구가 클래스를 실행한다. 
- 실행하는 클래스를 바꿀 때마다 `build.gradle`에 적어줘야 한다.

```gradle
application {
    // Define the main class for the application.
    mainClassName = 'com.eomcs.pms.App'
}
```

- 그러면 귀찮지 않나?? => 실무에서 아무리 많은 파일을 만들어도 실행하는 클래스는 하나밖에 없음(main)!

## 3. 자바 프로젝트 빌드하기

- **일반 사용자**에게 **배포할 파일**을 만든다
- 우리가 만든 프로젝트를 제품으로 패킹하는 과정


`gradle build`

### 빌드 절차
- 소스 파일 컴파일
- 단위 테스트 수행
- .jar 아카이브 파일 생성
- 실행과 관련된 파일 생성
- .tar, .zip  배포 파일 생성

빌드하면 다음과 같이 `/build/distributions/`에 배포파일이 생긴다.

```
├── build
│   ├── distributions
│   │   ├── bitcamp-java-project.tar  <== Unix/Linux 사용자에게 배포하는 파일
│   │   └── bitcamp-java-project.zip  <== Windows 사용자에게 배포하는 파일
```

배포 파일의 압축을 플고 그 안의 스크립트 파일을 실행해보자.

```console
➜  distributions git:(master) ✗ tar -xvf *.tar  <== 압축파일 푸는 명령어 
➜  distributions git:(master) ✗ cd bitcamp-java-project 
➜  bitcamp-java-project git:(master) ✗ tree
.
├── bin
│   ├── bitcamp-java-project
│   └── bitcamp-java-project.bat
└── lib
    ├── bitcamp-java-project.jar
    ├── checker-qual-2.11.1.jar
    ├── error_prone_annotations-2.3.4.jar
    ├── failureaccess-1.0.1.jar
    ├── guava-29.0-jre.jar
    ├── j2objc-annotations-1.3.jar
    ├── jsr305-3.0.2.jar
    └── listenablefuture-9999.0-empty-to-avoid-conflict-with-guava.jar

2 directories, 10 files
➜  bitcamp-java-project git:(master) ✗ cd bin 
➜  bin git:(master) ✗ ./bitcamp-java-project  <== 스크립트 파일 실행
Hello world.
```
- 이때 Windows os의 경우 bat파일을 실행한다. 
- 특히 우리가 만든 app.class 가 lib안의 bitcamp-java-project.jar에 있다.
- 여기에 있는 app.class를 실행하는 명령어가 bin에 있는 거!!! => 이게 바로 **shell script**이다.
- 이 스크립트는 고객 컴퓨터가 어떤지 모르니까 모든 경우의 수를 다 따져봐서 실행할 수 있게 만들어졌다.
- 개발자라면 그냥 jvm의 java 명령어로 실행할 수 있겠지만 빌드는 일반 사용자를 위해 배포파일을 만드는 과정이다.
- `gradle clean`: 빌드한 걸 삭제하는 명령어