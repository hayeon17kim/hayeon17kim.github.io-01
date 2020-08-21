---
title: ":yellow_heart: JavaScript 함수지향: 함수호출"
categories: TIL
tags: [ TIL ]
toc: true
---



## 함수호출

자바스크립트에서 함수는 일종의 객체이다. 객체는 속성과 메서드를 가지고 있다. 함수는 객체이기 때문에 메서드를 가지고 있다. 이 메서드는 자바스크립트가 기본적으로 제공하는 내장된 메서드이다. `func.apply()`, `func.call()` 등이 있다.

```javascript
// 함수를 호출하는 기본적인 방법
function func() {}
func();
```

JavaScript는 함수를 호출하는 특별한 방법을 제공한다. 위에서 func는 Function이라는 객체의 인스턴스이다. 따라서 func는 객체 Function이 가지고 있는 메서드들(`func.apply()`, `func.call()`을 상속하고 있다. 

```console
> sum.apply
function apply() { [native code] }
```



```javascript
function sum(arg1, arg2) {
    return arg1 + arg2;
}
sum(1,2);
sum.apply(null, [1,2]);
// 두 개는 같다.
```

함수 sum은 Function 객체의 인스턴스이다. 따라서 객체 Function의 메서드 apply를 호출할 수 있다. apply는 는 두 개의 인자를 가질 수 있는데, 첫 번째 인자는 함수(sum)가 실행될 맥락이다. 두 번째 인자는 배열인데, 이 배열에 담겨 있는 원소가 함수(sum)의 인자로 순차적으로 대입된다. Function.call은 사용법이 거의 비슷하다. 

sum 첫번째 인자는 apply의 두번째 인자로 전달된 배열의 첫번째 값..

apply란? 함수를 호출할 때 sum(1,2)라고 호출할 수도 있지만 sum.apply(null, [1,2])라고 호출할 수도 있다. 



### apply를 사용하는 구체적인 이유

```javascript
o1 = {val1:1, val2:2, val3:3}
o2 = {val1:10, val2:50, val3:100, v4:25}

function sum() {
    //var this = o1 or o2 와 같다. 
    var _sum = 0;
    for (name in this) {
        _sum += this[name];
    }
    return _sum;
}




alert(sum.apply(o1)); // 6
// 실행되는 그 순간에는 o1이라는 객체의 sum이라는 메서드가 되는 것(o1.sum)
alert(sum.apply(o2)); // 185
// o2.sum
```

- 객체 Function의 메서드 apply의 첫번째 인자는 함수가 실행될 맥락이다.
- `sum.apply(o1)`는 함수 sum을 객체 o1의 메서드로 만들고 sum을 호출한 후에 sum을 삭제한다. (실행결과가 조금 다르지만)

```javascript
o1.sum = sum;
alert(o1.sum());
delete o1.sum();
```

```javascript
function sum() {
    var _sum = 0;
    for (name in this) {
        if(typeof this[name] !== 'function')
            _sum += this[name];
        return sum;
    }
}
// 이렇게 해줌으로서 실행결과를 갖게 만들 수 있다.
// 이게 불편할 경우 apply를 사용하여 호출할 수 있다.
// 호출되는 함수의 this값을 프로그래밍적으로 변경해서 마치 sum이 o1의 함수인 것처럼 사용할 수 있는 거
```



- sum이 o1 소속의 메서드가 된다는 것? => 함수 sum에서 this의 값이 전역객체가 아니라 o1이 된다는 의미이다. 일반적인 객체지향 언어에서는 하나의 객체에 소속된 함수는 그 객체의 소유물이 된다. 하지만 JavaScript에서 함수는 독립적인 객체로서 존재하고, apply나 call 메서드를 통해서 다른 객체의 소유물인 것처럼 실행할 수 있다.
- 만약 apply의 첫 번째 인자로 null을 전달하면 apply가 실행된 함수 인스턴스는 전역객체(브라우저에서는 window)를 맥락으로 실행되게 된다. 




