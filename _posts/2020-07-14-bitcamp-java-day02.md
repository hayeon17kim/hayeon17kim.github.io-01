---
title: "비트캠프 자바 과정 2일차"
related: true
layout: splash
classes:
  - landing
  - dark-theme
categories: Java

---
# Day02 
>  2020-07-14

## Project Overview



![process](https://user-images.githubusercontent.com/50407047/87386180-dd073180-c5da-11ea-9942-0d4a6e58dc3c.jpg)



[git]

- 형상관리 도구
  - CVS
  - Subversion
  - Git
    - Git을 전문적으로 서비스해주는 사이트가 `github.com` (MS 인수)



[gradle]

- `.war`: web archive(보관): 웹 프로젝트를 하나의 파일로 패킹
  - .zip으로 변경 후 압축 풀어도 됨
  - 제품 한 상자로 패킹한 게 .war
- `.jar` 
- `.tar` : 백업하기 위해 한 파일로 만든 것 => 풀으면 디렉토리와 파일이 생김
- `.tar.gz`: tar로 묶은 파일을 압축


- `gradle build`
- `gradle clean` : 빌드한 것 삭제
- `라이브러리`: 다른 개발자들이 미리 작성한 명령어들의 모음
  - 뭘 다운받아야 하는지는 `package.json`에 적혀 있음
    - `npm install` 를 실행하면 현재 폴더에서 package.json 파일에 따라 라이브러리들을 설치함
    - node_modules 폴더가 생성되고 그 안에 라이브러리들이 생성됨
  - JS는 npm이라는 패키지 매니저가 있음
    - npm: node package manager
    - node: interpreter, compiler와 vm 비교를 위해 nodejs 사용해 볼 것임
- npm install => gradle build



1. 프로젝트 다운 (git clone)
2. npm install (라이브러리 설치)
3. gradle build (war 생성)



build 과정(gradle은 이걸 순차적으로 실행시켜줄 뿐, 과정에서 필요한 jdk 꼭 필요)

- compile (=> javac.exe)
- test (=> java.exe)
- upload 
- ... 


웹서버가 war 파일을 실행할 수 있도록 app을 웹서버에 **배치(Deploy)**한다



[MariaDB]

- 과정
  - 다운로드 및 설치
  - 사용자 생성
  - 데이터베이스 생성
  - 사용자가 사용할 데이터베이스의 권한 설정
  - 테이블 생성
  - 예제 및 마스터 데이터 입력

- 특징
  - 실무에서 많이 씀
  - google, facebook 에서 사용
  - My-SQL과 99퍼센트 호환
  - 포트번호: 3306 => 다른 곳과 통신하기 위해 고유번호
    - 서버쪽 포트번호는 중복되면 안됨
  - 서버 설치하면 클라이언트도 함께 설치 mysql client application software



### App 실행 방식 (Interpreter 방식 VS Compiler 방식)

![app 실행 방식 비교](https://user-images.githubusercontent.com/50407047/87401971-d76b1500-c5f5-11ea-8b33-86d91e202ca1.jpg)

- **Interpreter 방식**

  - **매번** interpreter가 **실행할 부분**만 문법 검사 후 실행: 속도 느림

  - 소스코드 노출

  - 실행할 때마다 Interpreter 필요

  - CPU와 OS에 비종속적

    - 원본 명령어를 해석기로 실행

  - ex) js(js의 해석기는 NodeJS), py, php

  - 장단점

    - 장점: 소스파일과 Interpreter만 있다면 바로 실행 가능
- 단점: 오류가 있음에도 불구하고 실행할 때만, 실행하는 부분만 문법검사하여 오류가 있는지 모름
      - 작은 프로젝트는 interpreter 방식 좋은데, 큰 프로젝트에서는 좋지 않음

```javascript
var lang = "english";

if (lang == "english")
    console.log("Hello,world!");	//바뀔 부분
else
    console.log('안녕하세요!');
```

위의 코드에서

```javascript
var lang = "english";

if (lang == "english")
    console.leg("Hello,world!");	//바뀐 부분
else
    console.log('안녕하세요!');
```

위와 같이 `console.leg`로 바뀐다면 문법에 맞지 않지만, 해당 부분은 **실행되지 않는 부분이기 때문에 interpreter가 문법검사를 하지 않는다.** 



- **Compiler 방식**

![컴파일러 방식의 교차 불가능성](https://user-images.githubusercontent.com/50407047/87406725-1603ce00-c5fc-11ea-8692-9c1b60a4441d.jpg)

  - 한번 compiler가 기계어로 번역하고 그 변환된 기계어로 실행: 속도 빠름
  - 소스코드 보호
  - 실행할 때마다 Interpreter 불필요
  - CPU와 OS에 종속적
    - CPU와 OS에 맞춰서 기계어로 번역함
    - CPU가 바뀌면 이 CPU에 맞춰서 다시 Compile 해야 함
  - ex) C
  - 기계어는 CPU에 따라 달라짐
  - Intel CPU의 기계어는 같지만 Windows와 Linux는 기계어를 배치하는 구조(포맷)가 다름. so CPU가 같아 기계어가 같다고 해도 운영체제가 다르면 실행 못함

  

###### Windows에 Compiler가 없는 이유

  - UNIX 
    
      - 컴파일러 포함 O
  - 전문가용
    
    - a.c (source) = Compile => a.exe (실행 파일)
- 옛날에는 하드웨어마다 CPU가 달랐음
    - 컴파일을 하면 각 CPU에서만 실행할 수 있는 파일이 만들어지기 때문에, 원본 source code 자체를 공유하는 것이 당연시되었음
  - Windows 
      - 컴파일러 포함 X
      - 일반인용: PC(Personal Computer) 보급 이후 사용된 운영체제 (예전에는 DOS)
      - Intel CPU => 컴파일한 exe 파일만을 복사해서 사용 가능
        - 소스코드 공유X
        - ex파일 공유O
        - so, 소수의 전문가를 위한 Visual Studio 유료로 판매
      - but 무료 컴파일러도 있음 (gcc)

  

#### 직접 컴파일 방식을 체험해보자.

  .c 파일을 만들어 주자.

  ```powershell
  #include <stdio.h>
  
  int main() {
      printf("Hello, world");
      printf("안녕하세요!");
      return 0;
  }
  ```

  

  gcc를 설치하고, 

  powershell에 다음과 같이 명령어를 친다.

  ```powershell
  PS C:\Users\bitcamp\bitcamp-workspace> gcc -o hello.exe hello.c
  ```

  `-o`: 만들고자 하는 목적 파일

  ```powershell
  PS C:\Users\bitcamp\bitcamp-workspace> dir
  
  
      디렉터리: C:\Users\bitcamp\bitcamp-workspace
  
  
  Mode                LastWriteTime         Length Name
  ----                -------------         ------ ----
  -a----     2020-07-14   오후 4:13          48434 a.exe
  -a----     2020-07-14   오후 4:12            114 hello.c
  -a----     2020-07-14   오후 4:14          48434 hello.exe	// 컴파일로 생성된 파일
  -a----     2020-07-14   오후 3:13             63 hello.js
  -a----     2020-07-14   오후 3:18            125 hello2.js
  ```

  다음과 같이 컴파일된 hello.exe가 생성되었다. (기계어로 변환됨)

  이 과정에서 문법 검사를 한다.

```powershell
PS C:\Users\bitcamp\bitcamp-workspace> ./hello                     
Hello, world?덈뀞?섏꽭??
```

컴파일된 파일을 실행하면 다음과 같이 나온다 (한글은 깨진다..)

그러나, 이 exe 파일은 컴파일한 환경인 Intel CPU와 Windows 10에 **맞춰 컴파일되었기 때문에**, Mac이나 Linux에서는 실행되지 않는다.

수정사항이 있으면 다시 컴파일해야 하는데, 수정사항을 저장만 하면 되는 interpreter 방식과 다르다.



##### 안드로이드 os 는?

- Playstore에 **중간언어**로 컴파일한 파일(이미 문법검사 끝난 App)을 올림
- 핸드폰에 다운받을 때 **최종번역**된 App 다운받는다.
- 따라서, CPU가 다른 핸드폰에서 다 실행이 되는 것.



#### Hybrid 방식: Java

>  Java: "Write Once, Run Anywhere"



![Java의 Hybrid 실행 방식](https://user-images.githubusercontent.com/50407047/87406765-29169e00-c5fc-11ea-9136-0a9b65f2ff7f.jpg)

명령어를 관리하기 쉽게 분류

- classfication => 독립적 단위: 객체
  - Object Oriented Program(OOP)
  - C => C++
- JVM
  - App -> JVM -> OS
  - Just In Time (JIT): 예전 안드로이드 방식
    - 실행할 때 시간 걸림
  - Ahead of Time(AOT): 실행 앞서 미리 컴파일 다 함 (지금 안드로이드 방식). so 설치할 때 시간이 걸림



보통 

- 컴파일 과정을 거쳐서 기계어로 바꿈

- 자바는 기계어에 흡사한 class파일을 만들어서 interpreter로 해석



Specification

- 이 정의서(명세서)에 따라 작성해야 함
- [Java Language and Virtual Machine Specification](https://docs.oracle.com/javase/specs/)
  - 모든 교재는 이 Specification을 기반으로 함
  - Java는 여기에 따라 만듦
