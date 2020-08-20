---
title: ":yellow_heart: JavaScript 객체지향: 객체지향 프로그래밍"
categories: TIL
tags: [ TIL ]
toc: true
---



# 객체지향

객체지향 프로그래밍은 크고 견고한 프로그램을 만들기 위한 노력의 산물이다. 그러나 JavaScript의 객체지향은 다른 주류 객체지향 언어(Java, C++)과 사뭇 다르다. 모든 처리의 중심에 함수를 두는 자바스크립트를 공부하다 보면 객체지향을 이렇게도 추구할 수 있구나 하는 놀라움을 느낄 수 있다.



### 객체지향 프로그래밍

- 좀 더 나은 프로그램을 만들기 위한 프로그래밍 패러다임
- 로직을 상태(state)와 행위(behave)로 이루어진 객체로 만들고, 이 객체들을 레고 블럭처럼 조립해서 하나의 프로그램을 만드는 것
- 객체를 만드는 것
- **객체는 변수와 메서드를 그룹핑한 것**이다. 



### 문법과 설계

객체지향 프로그래밍 교육은 크게 문법과 설계로 구분된다.

#### 문법

- 객체지향을 편하게 할 수 있도록 언어가 제공하는 기능을 익히는 것
- 문법을 이해하고 숙지해야 객체를 만들 수 있다. 
- 객체를 만드는 법에 대한 학습 



#### 설계

- 좋은 객체를 만드는 법
- 설계를 잘 하는 법
- 좋은 설계는 현실을 잘 반영해야 한다. 현실은 복잡하지만 그 복잡함 전체가 필요한 것은 아니다. **복잡함 속에서 필요한 관점만을 추출**하는 것, 즉 **추상화**가 필요하다.
- 현실 => 소프트웨어
- 지하철 노선도가 디자인의 추상화라면 프로그램을 만드는 것은 소프트웨어의 추상화라고 할 수 있다.
- 객체지향 프로그래밍: 좀 더 현실을 잘 반영하기 위한 노력의 산물이다. 단순히 객체 지향의 문법을 이용해서 객체를 만든다고 달성되는 것이 아니다. 고도의 추상화 능력이 필요하다. 좋은 설계는 문법을 배우는 것보다 훨씬 어려운 일이다. 
- 일단은 지식부터 익히자. 



### 부품화

- 객체지향: 부품화의 정점
- 메서드는 부품화의 예이다. 메서드의 기본 취지는 연관되어 있는 로직들을 결합해서 메서드라는 완제품을 만드는 것이다. 이 메서드를 부품으로 하나의 완제품인 독립된 프로그램을 만드는 것이다. 메서드를 사용하면 코드의 양을 극정으로 줄일 수 있고, 필요한 코드를 찾기도 쉽고, 문제의 진단도 빨라진다.
- 그러나 프로그램이 커지면 많은 메서드가 생겨나고 메서드와 벼눗를 관리하는 것이 점점 어려워지기 시작한다. 메서드는 프로그래밍의 역사에서 중요한 도약이었지만, 이 도약이 성숙해지면서 새로운 도약지점이 보이기 시작한 것이다.
- 그 도약 중 하나가 객체지향 프로그래밍이다. 이것의 핵심은 연관된 메서드와 그 메서드가 사용하는 변수들을 분류하고 그룹핑하는 것이다. 이렇게 그룹핑한 대상이 객체(Object)이다. 이를 통해서 더 큰 단위의 부품을 만들 수 있게 되었다. 

#### 은닉화, 캡슐화

부품화라는 목표를 달성하기 위해서는 그것이 어떻게 만들었는지 모르는 사람도 그 부품을 사용하는 방법만 알면 쓸 수 있어야 한다. 내부의 동작 방법을 단단한 케이스 안으로 숨기고 사용자에게는 그 부품의 사용방법만을 노출하고 있는 것이다. 이러한 컨셉을 정보의 은닉화(Information Hiding) 또는 캡슐화(Encapsulation)라고 부른다.  자연스럽게 사용자에게는 그 부품을 사용하는 방법이 중요한 것이 된다.



#### 인터페이스

잘 만들어진 부품이라면 부품과 부품을 서로 교환할 수 있어야 한다. 인터페이스란 이질적인 것들이 결합하는 것을 막아주는 역할도 한다. (모니터 입장에서는 컴퓨터가, 컴퓨터 입장에서는 모니터가 어떤 식으로 만들어졌는지는 신경 쓰지 않는다. 각각의 부품은 미리 정해진 약속에 따라서 신호를 입, 출력하고, 연결점의 모양을 표준에 맞게 만들면 된다.)즉 인터페이스는 부품들 간의 약속이다. 이러한 약속을 프로그래밍적으로 어떻게 구현할까?



#### 복제와 상속



## 생성자와  new

자바스크립트는 어떠한 객체지향 언어와도 같지 않다. Prototype-based programming. 

### 객체

객체 만드는 방법1: 만드는 과정 분산

```javascript
var person = {}
person.name = 'egoing';
person.introduce = function() {
    return 'My name is ' + this.name;
}
document.write(person.introduce());
```

객체 만드는 방법2: 정의할 때 값을 셋팅

```javascript
var person = {
    'name' : 'egoing',
    'introduce' : function() {
        return 'My name is ' + this.name;
    }
}
document.write(person.introduce());
```

그렇다면 여러 개의 객체를 만들 때는 어떻게 해야 할까?

```javascript
var person1 = {
    'name' : 'egoing',
    'introduce' : function() { return 'My name is ' + this.name}
}
var person2 = {
    'name' : 'monica',
    'introduce' : function() { return 'My name is ' + this.name}
}
```

위와 같이 객체의 정의를 반복해야 한다. 어떻게 하면 객체의 구조를 재활용할 수 있을까? => 생성자를 사용하자!



### 생성자

생성자(constructor)는 객체를 만드는 역할을 하는 함수이다. 자바스크립트에서 함수는 단순히 재사용 가능한 로직의 묶음이 아니라 **객체를 만드는 창조자**라고 할 수 있다. 

```javascript
function Person(){}
var p0 = Person(); 
console.log(p0) // undefined

var p0 = new Person(); 
console.log(p0) // Person {}
// new와 함께 함수 호출: 비어있는 객체를 만들고 객체를 반환한다.
```



```javascript
function Person(){}
var p = new Person();
p.name  = 'egoing';
p.introduce = function() {
    return 'My name is ' + this.name;
}
document.write(p.introduce);
```

함수를 호출할 때 new를 붙이면 새로운 객체를 만든 후에 이를 리턴한다. 위의 코드는 새로운 객체를 변수 p에 담았다. 여러 사람을 위한 객체를 만든다면 다음과 같이 코드를 작성해야 한다.

```javascript
function Person(){}

var p1 = new Person();
p1.name = 'egoing';
p1.introduce = function() {
    return 'My name is ' + this.name;
}
document.write(p1.introduce() + "<br />");

var p2 = new Person();
p2.name = 'egoing';
p2.introduce = function() {
    return 'My name is ' + this.name;
}
document.write(p2.introduce());
```

별로 개선된 것이 없다.

```javascript
function Person(name) {
    this.name = name;
    this.introduce = function() {
        return 'My name is ' + this.name;
    }
}
var p1 = new Person('egoing');
document.write(p1.introduce() + "<br />");

var p2 = new Person('monica');
document.write(p2.introduce() + "<br />");
```

생성자 내에서 객체의 프로퍼티를 정의하고 있다. 이러한 작업을 초기화라고 한다. 이를 통해서 코드의 재사용성이 높아졌다. 생성자 함수는 일반 함수와 구분하기 위해서 첫글자를 대문자로 표시한다.



#### 자바스크립트 생성자의 특징

일반적인 객체지향 언어에서 생성자는 클래스의 소속이다. 하지만 자바스크립트에서 객체를 만드는 주체는 함수이다. 함수에 new를 붙이는 것을 통해서 객체를 만들 수 있다는 점은 자바스크립트에서 함수의 위상을 암시하는 단서면서 또 자바스크립트가 추구하는 자유로움을 보여주는 사례라고 할 수 있따. 

