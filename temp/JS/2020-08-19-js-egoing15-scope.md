---
title: ":yellow_heart: JavaScript 함수지향: 유효범위"
categories: TIL
tags: [ TIL ]
toc: true
---

# 함수지향

함수형 언어로서의 자바스크립트의 면모를 다룬다. 자바스크립트의 핵심적인 도구는 함수고 매우 강력하다. 자바스크립트에서 함수는 객체를 이해하는 데 가장 중요한 기초를 이룬다.

## 유효범위

- 유효범위(scop)는 변수의 수명을 의미한다.
- 전역변수는 애플리케이션 전역에서 접근이 가능한 변수이다.
- 지역변수의 유효범위는 함수 안이고 전역변수의 유효범위는 애플리케이션 전역이다.
- 같은 이름의 지역변수와 전역변수가 동시에 정의되어있다면 지역변수가 우선한다. 
- var를 사용하지 않은 지역변수는 전역변수가 된다. 
  - 물론 var를 사용하여 선언한 후 그 값에 값을 할당할 때는 var를 사용하지 않고 할당한다. 그래도 전역변수가 되지 않는다.
- 전역변수는 가능한 사용하지 말자.
  - 함수 안에서 전역변수를 사용하고 있는데 누군가에 의해서 전역변수의 값이 달라지면 함수의 동작도 달라지게 된다. => 버그의 원인, 함수를 다른 애플리케이션에 이식하기 어렵다. (함수의 핵심은 로직의 재활용인데..)

```javascript
var vscope = 'global';
function fscope() {
    alert(vscope);
}
fscope(); // global
```

```javascript
var vscope = 'global';
function fscope() {
    var vscope = 'local';
    alert('함수안 ' + vscope);
}
fscope(); 
alert('함수밖 ' + vscope) 
// 함수안 local
// 함수밖 global
```

```javascript
var vscope = 'global';
function fscope() {
    vscope = 'local';
    alert('함수안 ' + vscope);
}
fscope();
alert('함수밖 ' + vscope);
// 함수안 local
// 함수밖 local
```



### 유효범위의 효용

```javascript
// 지역변수 사용
function a () {
    var i = 0; // 지역변수
}
for (var i = 0; i < 5; i++) { //전역변수
    a();
    document.write(i);
}
// 01234
```

```javascript
// 전역변수 사용
function a () {
    i = 0; // 전역변수
}
for (i = 0; i < 5; i++) { // 전역변수
    a();
    // i가 계속 0으로 초기화되어 i는 항상 5보다 작게됨
    // 따라서 무한루프 발생
    document.write(i);
}
// 무한루프
```

예전에는 변수를 선언하는 게 굉장히 조심스러웠다. 충돌이 일어나기 쉽기 때문이다. 옛날에는 데이터가 하나였지만 =>  파일 쪼개짐  => 디렉토리 

지역변수/전역변수/유효범위 개념은 이름의 충돌, 로직의 충돌을 고민한 결과. 



### 전역변수의 사용

불가피하게 전역변수를 사용해야 하는 경우는 하나으 ㅣ객체를 전역변수로 만들고 객체의 속성으로 변수를 관리하는 방법을 사용한다.

```javascript 
var MYAPP = {} 
// 전역변수 1개에 나머지 다른 전역 변수들은 객체의 속성으로 넣음
MYAPP.calculator = {
    'left' : null,
    'right' : null
}
MYAPP.coordinate = {
    'left' : null,
    'right' : null
}

MYAPP.calculator.left = 10;
MYAPP.calculator.right = 20;

function sum() {
    return MYAPP.calculator.left + MYAPP.calculator.right;
}
document.write(sum());
```

익명함수를 호출함으로서 전역 변수를 사용하지 않을 수 있다. 자바스크립트에서 로직을 모듈화하는 일반적인 방법이다. 

```javascript
// 전역 변수 1개도 만들고 싶지 않을 때 
// 함수를 정의하고 바로 호출
(function() {
    var MYAPP = {} // 이 함수의 지역변수
    MYAPP.calculator = {
        'left' : null.
        'right' : null
    }
    MYAPP.coordinate = {
        'left' : null,
        'right' : null
    }
   	MYAPP.calculator.left = 10;
    MYAPP.calculator.right = 20;
    function sum() {
        return MYAPP.calculator.left + MYAPP.calculator.right;
    }
    document.write(sum());
}())
```



### 유효범위의 대상(함수)

- 자바스크립트는 함수에 대한 유효범위만을 제공한다.

- 즉, 자바스크립트의 지역변수는 함수에서만 유효하다.

```javascript
for (var i = 0; i < 1; i++) {
    var name = 'coding everybody';
}
alert(name);
```

자바에서는 블록에 대한 유효범위를 제공한다. 

```java
for (var i = 0; i < 1; i++) {
    String name = 'coding everybody'; //지역변수
}
System.out.println(name); //error
// for문의 지역변수인데 밖에서 호출하고 있다.
// 존재하지 않는 변수 접근
```



### 정적 유효범위

자바스크립트는 함수가 선언된 시점에서의 유효범위를 갖는다. 이것을 정적 유효범위(static scoping), 혹은 렉시컬(lexical scoping)이라고 한다. 클로저 개념과 연결된다. 

```javascript
var i = 5;

function a() {
    var i = 10;
    b();
}

function b() {
    document.write(i); 
    // document.write에 지역변수 i 가 있는지 찾는다.
	// 없다면 함수 b에 지역변수 i가 있는지 찾는다.
	// 없다면 전역변수 i가 있는지 찾는다. 
}

a();
// 함수 b를 호출한 것은 함수 a이지만 함수 a에서 i를 찾지는 않는다.
// 이는 자바스크립트의 렉시컬 스코핑의 성격 때문이다. 
```

- 사용될 때가 아니라 정의될 때의 전역변수가 사용되게 된다. 
- b가 누구에게 사용될 지 모르니까 정의된 그 시점의 변수를 바라보게 되면 누가 사용하더라도 똑같기 때문에 정적 유효범위라 부른다.


