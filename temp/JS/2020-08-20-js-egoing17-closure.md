---
title: ":yellow_heart: JavaScript 함수지향: 클로저"
categories: TIL
tags: [ TIL ]
toc: true
---

# 클로저

- JavaScript의 함수와 다른 언어의 함수의 차이점: 함수가 값이 될 수 있다.



### 함수의 용도

함수는 first-class citizen(object)(변수, 매개변수, 리턴값으로 사용할 수 있는 object)이다.

- 함수는 객체의 값으로 포함될 수 있다.
  - 객체의 속성 값으로 담겨진 함수를 메서드(method)라고 부른다.

```javascript
a = {
    b: function(){}
    //key: value
    // key는 객체 안에서 일종의 변수 역할을 한다. (value를 담는 그릇)
}
```

- 다른 함수의 인자로 전달될 수 있다.

```javascript
function cal(func, num) {
    return func(num);
}

function increase(num) {
    return num + 1;
}

function decrease(num) {
    return num - 1;
}

alert(cal(increase, 1));
alert(cal(decrease, 2));
```

- 다른 함수의 리턴값이 될 수 있다.

```javascript
function cal(mode) {
    var funcs = {
        'plus' : function(left, right){return left + right},
        'minus' : function(left, right)[return left - right]
    }
    return funcs[mode];
}
alert(cal('plus')(2,1)); // 1
// cal('plus') == function(left, right){return left + right}
// 괄호가 나온다는 것은 함수가 호출된다는 의미

alert(cal('minus')(2,1)); // 2
```

- 배열의 값으로 사용할 수 있다.

```javascript
var process = [
    function(input) { return input + 10; },
    function(input) { return input * input; },
    function(input) { return input / 2; }
];
var input = 1;
for (var i = 0; i < process.length; i++) {
    input = process[i](input);
    // 괄호 -> 호출
}
console.log(input); // 60.5 
```



### 콜백

- 값으로 전달된 함수는 호출되어 함수의 동작을 완전히 바꿀 수 있다. 

```javascript
// 포맷을 지켜줘야 함
function sortNumber (a, b) {
    return b - a;
}

var numbers = [20, 10, 9, 8, 7, 6, 5, 4, 3, 2, 1];
console.log(numbers.sort(sortNumber));
// sortNumber가 콜백함수
// 값으로 함수(콜백)를 넘겨주어 오리지널 함수의 동작 방법을 완전히 바꿀 수 있다.
// 객체.메서드()
// sort(): 내장 메서드
```

- `array.sort(sortfunc)`
  - 옵션인 `sortfunc`로 두 개의 인자를 전달하는데 리턴값에 따라서 선후를 판단 (리턴값의 부호)
- 인자로 전달된 함수 sortNumber의 구현에 따라서 sort의 동작 바업이 완전히 바뀌었다.



### 비동기 처리

콜백은 비동기처리에도 유용하게 사용된다. 시간이 오래 걸리는 작업이 있을 때 이 작업이 완료된 후에 처리해야 할 일을 콜백으로 지정하면 해당 작업이 끝났을 때 미리 등록한 작업을 실행하도록 할 수 있다. 

- `Ajax`(Asynchronous Javascript and XML)

- 두번째 인자로는 함수를 전달하도록 약속되어 있는데, 서버랑 통신이 끝났을 때 호출되도록 약속되어 있다. 함수에는 인자값을 전달하도록 약속되어 있는데, 서버의 객체 정보를인자로. 함수를 매개변수의 값으로 전달하는 것을 통해 get 메소드의 동작 방법을 완전히 바꿔놓을 수 있다. 

datasource.json.js

```javascript
{"title:"JavaScript", "author":"egoing"}
```

demo1.html

```HTML
<! DOCTYPE html>
<html>
    <head>
        <script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
    </head>
    <body>
        <script type="/text/javascript">
        $.get('./datasource.json.js', function(result){
        console.log(result);
        }, 'json')
        </script>
    </body>
</html>
```



# 클로저

클로저(closure)는 내부함수가 외부함수의 맥락(context)에 접근할 수 있는 것을 가리킨다.

### 내부함수

- 내부함수는 외부함수의 지역변수에 접근할 수 있다.

```javascript
function outter() {
    var title = 'coding everybody'; // 외부함수의 지역변수
    function inner() {
        alert(title);
    }
    // var inner = function(){} 와 같다!!!
    // 그 함수 안에서만 사용하는 함수라면 안에다 넣는 게 좋음 (응집성)
}
outer(); // coding everybody
```



### 클로저

- 외부함수의 실행이 끝나서 외부함수가 소멸된 이후에도 내부함수가 외부함수의 변수에 접근할 수 있다. 

```javascript
function outter() {
    var title = 'coding everybody';
    return function() {
        alert(title);
    }
    // return 했다는 것은 그 함수(outter)는 생을 마감했다는 뜻
    // 외부함수는 이미 죽었는데 외부함수로 인해 파생된 내부함수에서 이미 죽은 외부함수의 변수에 성공적으로 접근하고 있다. 
}
inner = outter();
inner();
```

- 즉 내부함수는 외부함수의 지역변수에 접근할 수 있을 뿐만 아니라 외부함수의 실행이 끝나서 소멸된 이후에도 내부함수가 외부함수의 변수에 접근할 수 있다는 것이다. 
- outter를 호출한 결과인 이름 없는 함수가 변수 inner에 담긴다. 보통 여기서 outter 함수는 실행이 끝났기 때문에  이 함수의 지역변수는 소멸되는 것이 자연스럽다. 그러나 inner를 실행했을 때 coding everybody가 출력된 것은 지역변수 title이 소멸되지 않았다는 것을 의미한다.
- 클로저란 내부함수가 외부함수의 지역변수에 접근할 수 있고, **외부함수는 외부함수의 지역변수를 사용하는 내부함수가 소멸될 때까지 소멸되지 않는 특성**을 의미한다.



### private variable

```javascript
function factory_movie(title) {
    return {
        get_title : function () { //get_title 메서드
            return title;
        },
        set_title : function (_title){ //set_title 메서드
            title = _title;
        }
        // 객체 안에 함수가 정의되어 있음
        // 메서드는 factory_movie 함수의 내부함수
        // 외부함수의 지역변수에 접근할 수 있다.
        // 매개변수는 함수 안에서 지역변수로 사용된다. (title)
        // 그래서 내부함수에서는 title에 접근할 수 있다. 
    }
}

ghost = factory_movie('Ghost in the shell');
// ghost와 matrix는 
// 메서드를 담고 있는 컨테이너인 객체를 리턴한다는 것은 같지만
// 객체가 실행된 맥락이 다르다.
// 즉, 객체가 내부적으로 접근하는 외부함수의 지역변수인 title의 값은 다르다.
matrix = factory_movie('Matrix');

alert(ghost.get_title()); //Ghost in the shell
alert(matrix.get_title()); //Matrix

ghost.set_title('공각기동대');
// '공각기동대' => '_title' 인자로 전달된다.
// _title은 외부함수의 변수인 title을 바꾼다. 

alert(ghost.get_title()); //공각기동대
alert(matrix.get_title()); //Matrix

// 실행된 그 시점에서의 맥락, 외부함수의 지역변수에 접근할 수 있었다.
// 그 지역변수는 유지가 되고 있었다.
// ghost객체가 접근할 수 있는 title 값만 바꿀 뿐이지
// matrix가 접근할 수 있는 title의 값에는 어떠한 영향도 미칠 수 없다.

// 그래서 private 변수가 가능하다.
// private 변수?
// 메서드는 누구나 접근할 수 있는 public이다.
// 지역변수인 title은 get_title이나 set_title 메서드로만 접근 가능하다.
// => title 변수를 누가 수정한다고 해도 그 객체 맥락에 영향을 미치지 않는다.
// 약간 인스턴스 메서드랑 비슷한 것 같은데... 좀 더 이해 필요...
// 아무튼 캡슐화를 위해서 변수를 private으로 설정하고 메서드만 공개한다는 말같음
```



```javascript
// 이렇게 title 변수를 꽁꽁 숨겨두고
function factory_movie(title) { 
    return {
        // title 변수를 가져올 때는 get_title 메서드
        get_title: function() {return title;},
        // title 변수를 바꿀 때는 set_title 메서드 사용
        set_title: function(_title) {
            if(typeof _title === 'String'){
                title = _title
            } else {
                alert('제목은 문자열이어야 합니다.');
            }
        }
    }
}
ghost = factory_moveie('Ghost in the shell');
matrix = factory_movie('Matrix');

```

정리

- 클로저는 객체의 메서드에서도 사용할 수 있다. 위의 예제는 함수의 리턴값으로 객체를 반환하고 있다. 이 객체는 메서드 get_title과 set_title을 가지고 있다. 이 메서드들은 외부함수인 factory_movie의 인자값으로 전달된 지역변수 title을 사용하고 있다.
- 동일한 외부함수 안에서 만들어진 내부함수나 메서드는 외부함수의 지역변수를 공유한다. 마지막에 ghost.get_title(); 의 값이 '공각기동대'인 것은 set_title과 get_title 함수가 title의 값을 공유하고 있다는 의미이다.
- 외부함수가 실행될 때마다 새로운 지역변수를 포함하는 클로저가 생성되기 때문에 ghost와 matrix는 서로 완전히 독립적인 객체가 된다. (인스턴스????? 함수로 인스턴스 만들기?????? 클래스가 아니라??????)
- title의 값을 읽고 수정할 수 있는 것은 factory_movie 메서드를 통해서 만들어진 객체 뿐이다. JavaScript는 기본적으로 Private한 속성을 지원하지 않는데, 클로저의 이러한 특성을 이용해서 Private한 속성을 사용할 수 있게 된다.

- Private 속성은 객체의 외부에서 접근할 수 없는 외부에 감춰진 속성이나 메서드를 의미한다. 이를 통해서 객체의 내부에서만 사용해야 하는 값이 노출됨으로써 생길 수 있는 오류를 줄일 수 있다. 자바와 같은 언어에서는 이러한 특성을 언어 문법 차원에서 지원하고 있다.



### 클로저를 구현할 때 할 수 있는 실수

```javascript
var arr = []
for (var i = 0; i < 5; i++) { // 함수의 외부 변수의 값이 아니다.
    arr[i] = function() {
        return i;
    }
}
for (var index in arr) {
    console.log(arr[index]());
}
// 5 5 5 5 5 
```

```javascript
var arr = []
for (var i = 0; i < 5; i++) { 
    // 함수 호출될 때의 i값을 id라는 지역변수로 가지고 있음
    arr[i] = function(id) { // 외부함수를 만들어주고 id라는 매개변수로 i를 받음
        return function () { 
            return id;// 외부함수 지역변수인 id를 리턴하고 있다. (접근)
        }
    }(i); // 외부함수의 인자값으로 i를 넘겨준다.
}
for (var index in arr) {
    console.log(arr[index]);
}
// 0 1 2 3 4
```



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


