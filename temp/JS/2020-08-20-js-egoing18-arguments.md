---
title: ":yellow_heart: JavaScript 함수지향: arguments"
categories: TIL
tags: [ TIL ]
toc: true
---

# arguments

- arguments라는 객체는 변수에 담긴 숨겨진 유사배열이다. 이 배열에는 함수를 호출할 때 입력한 인자가 담겨있다. 
- arguments는 사실 배열은 아니다. 실제로는 arguments 객체의 인스턴스다.
- 저 안에는 사용자가 전달한 인자가 안에 들어가 있다. 이 객체를 통해서 사용자가 전달한 인자들에 접근할 수 있게 됨. `arguments.length`하면 sum

```javascript
// 가변 파라미터와 비슷한 느낌이군
function sum() {
    var i, _sum = 0;
    for (i = 0; i < arguments.length; i++) {
        document.write(i+' : '+arguments[i]+'<br />');
        _sum += arguments[i];
    }
    return _sum;
}
document.write('result : ' + sum(1,2,3,4));
```



### 매개변수의 수

- 매개변수와 관련된 두 가지 수가 있다.
  - `arguments.length`: 함수로 전달된 실제 인자의 수
  - `함수.length`: 함수에 정의된 인자의 수
- 사용처: 함수.length 와 arguments.length를 비교하여 다르면 오류나 경고를 띄울 수 있다.

```javascript
function zero() {
    console.log(
    	'zero.length', zero.length,
        'arguments', arguments.length
    );
}

function one(arg1) {
    console.log(
    	'one.length', one.length,
        'arguments', arguments.length
    );
}

function two(arg1, arg2) {
    console.log(
    	'two.length', two.length,
        'arguments', arguments.length
    );
}

zero(); // zero.length 0 arguments 0
one('val1', 'val2'); // one.length 1 arguments 2
two('val1') // two.length 2 arguments 1
```


