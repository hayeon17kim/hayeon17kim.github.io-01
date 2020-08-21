---
title: ":yellow_heart: JavaScript 객체지향: 함수와 this"
categories: TIL
tags: [ TIL ]
toc: true
---

# this

- `this`는 함수 내에서 함수 호출 맥락(context)를 의미한다. 
- 함수를 어떻게 호출하느냐에 따라서 this가 가리키는 대상이 달라진다.
- 함수와 객체의 관계가 느슨한 자바스크립트에서 `this`는 이 둘을 연결시켜주는 실질적인 연결점 역할을 한다.



## 함수호출

```javascript
function func() {
    if(window == this) {
        document.write("window === this");
    }
}
func(); // window === this
```

## 메서드 호출

객체의 소속인 메서드의 this는 해당 객체를 가리킨다.

```javascript
var o = {
    func : function() {
        if(o === this) {
            document.write("o === this");
        }
    }
}
o.func(); // o === this
```

## 생성자 호출

- 생성자는 빈 객체를 만든다. 이 객체 내에서 this는 만들어진 객체를 가리킨다.
- 생성자가 실행되기 전까지는 객체는 변수에도 발당될 수 없기 때문에 this가 아니면 객체에 대한 어떠한 작업도 할 수 없다.

```javascript
var funcThis = null;

function Func() {
    funcThis = this;
}

// 함수 호출
var o1 = Func();
if (funcThis === window) {
    document.write('window <br />'); // window
}

// 생성자 호출
var o2 = Func();
if (funcThis === o2) {
    document.write('o2 <br />'); // o2
}
```

```javascript
function Func() {
    document.write(o);
}
var o = new Func(); // undefined
```



## apply, call

- 함수의 메서드인 apply, call을 이용하면 this의 값을 제어할 수 있다.

```javascript
var o = {}
var p = {}
function func() {
    switch(this) {
        case o:
            document.write('o<br />');
            break;
        case p:
            document.write('p<br />');
            break;
        case window:
            document.write('window<br />');
            break;
    }
}
func(); //window
func.apply(o); //o
func.apply(p); //p
```





