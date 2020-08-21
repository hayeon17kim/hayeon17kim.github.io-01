---
title: ":yellow_heart: JavaScript 객체지향: 상속"
categories: TIL
tags: [ TIL ]
toc: true
---

# 상속

## 상속이란?

- 객체: 연관된 로직들로 이루어진 작은 프로그램
- 상속: 객체의 로직을 그대로 물려 받는 또 다른 객체를 만들 수 있는 기능을 말한다.
- 상속은 기존의 로직을 수정하고 변경해서 파생된 새로운 객체를 만들 수 있게 해준다.



```javascript
function Person(name) {
    this.name = name;
    this.introduce = function(){
        return 'My name is ' + this.name;
    }
}

var p1 = new Person('egoing');
document.write(p1.introduce() + "<br />");
// My name is egoing
```



```javascript
function Person(name) {
    this.name = name;
}
Person.prototype.name = null;
Person.prototype.introduce = function() {
    return 'My name is ' + this.name;
}
var p1 = new Person('egoing');
document.write(p1.introduce() + "<br />");
```



## 상속의 사용법

```javascript
function Person(name) {
    this.name = name;
}

Person.prototype.name = null;
Person.prototype.introduce = function() {
    return 'My name is ' + this.name;
}

function Programmer(name) {
    this.name = name;
}
Programmer.prototype = new Person();

var p1 = new Programmer('egoing');
document.write(p1.introduce() + "<br />");
```

Programmer 생성자의 prototype과 Person의 객체를 연결했더니 Programmer 객체도 메서드 Introduce를 사용할 수 있게 되었다. Programmer가 Person의 기능ㅇ르 상속하고 있는 것이다. 

## 기능의 추가

```javascript
function Person(name) {
    this.name = name;
}
Person.prototype.name = null;
Person.prototype.introduce = function() {
    return 'My name is ' + this.name;
}

function Programmer(name) {
    this.name = name;
}
Programmer.prototype = new Person();
Programmer.prototype.coding = function(){
    return "hello world";
}

var p1 = new Programmer('egoing');
document.write(p1.introduce() + "<br />");
document.write(p1.coding() + "<br />");
```

Programmer는 Person의 기능을 가지고 있으면서 Person이 가지고 있지 않은 기능인 메서드 coding을 가지고 있다.



