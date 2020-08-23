---
title: ":yellow_heart: JavaScript 객체지향: prototype"
categories: TIL
tags: [ TIL ]
toc: true
---

# prototype

## prototype

- 객체의 원형
- 함수는 객체이므로 생성자로 사용될 함수도 객체이다. 객체는 프로퍼티를 가질 수 있다. prototype이라는 프로퍼티는 그 용도가 약속되어 있는 특수한 프로퍼티이다.
- prototype에 저장된 속성들은 생성자를 통해서 객체가 만들어질 때 그 객체에 연결된다. 

> 프로토타입 기반 프로그래밍: 객체지향 프로그래밍의 한 형태로 클래스가 없고, 클래스 기반 언어에서 상속을 사용하는 것과 다르게 객체를 원형(프로토타입)으로 하여 복제의 과정ㅇ르 통해서 객체의 동작 방식을 다시 사용할 수 있다. 복제는 원래 있던 객체의 프로토타입의 행동을 복제하여 새 객체를 생성하는 과정을 거친다. 새 객체는 원본의 모든 특성을 가진다. 이 상태에서 새 객체를 수정할 수도 있다. 일반적인 클래스 기반 객체지향 언어에서 클래스와 인스턴스의 관계와 달리 프로토타입과 파생된 객체는 이 연결을 통해 프로토타입과 자식 객체가 메모리나 구조적 유사성을 가질 필요가 없다.

```javascript
function Ultra(){}
Ultra.prototype.ultraProp = true;

function Super(){}
Super.prototype = new Ultra();

function Sub(){}
Sub.prototype = new Super();

var o = new Sub();
console.log(o.ultraProp) // true
```

## prototype chain

생성자 Sub를 통해서 만들어진 객체 o가 Ultra의 프로퍼티 ultraProp에 접근 가능한 것은 prototype 체인으로 Sub와 Ultra가 연결되어 있기 때문이다. 

1. 객체 o에서 `ultraProp`을 찾는다.
2. 없다면 `Sub.prototype.ultraProp`을 찾는다.
3. 없다면 `Super.prototype.ultraProp`을 찾는다.
4. 없다면 `Ultra.prototype.ultraProp`을 찾는다.

property는 객체와 객체를 연결하는 체인의 역할을 하고, 이러한 관계를 prototype chain이라고 한다.

> Super.prototype = Ultra.prototype으로 하면 안된다. 이렇게 하면 Super.prototype의 값을 변경하면 그것이 Ultra.prototype도 변경하기 때문이다. `Super.prototype = new Ultra()`는 Ultra.prototype의 원혀애으로 하는 객체가 생성되기 때문에 new Ultra()를 통해 만들어진 객체에 변화가 생겨도 Ultra.prototype의 객체에는 영향을 주지 않는다.

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



