---
title: ":yellow_heart: JavaScript 객체지향: 생성자와 new"
categories: TIL
tags: [ TIL ]
toc: true
---

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


