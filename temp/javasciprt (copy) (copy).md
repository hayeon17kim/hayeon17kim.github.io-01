---
title: ":yellow_heart: JavaScript 기본: 객체"
categories: TIL
tags: [ TIL ]
toc: true
---

# 객체

서로 연관되어 있는 데이터를 담아내기 위한 그릇이라는 점에서 배열과 객체는 유사하다. 

배열은 데이터를 담으면 0, 1, 2라는 인덱스가 자동으로 붙는다. 반면 객체는 인덱스의 값으로 직접 원하는 데이터를 지정할 수 있다. 

## 객체 만들기

``` javascript
var grades = {'egoing': 10, 'k8805': 6, 'sorialgi': 80}
{Key: value}

// 객체 생성 방법 (1)
var grades = {};

// (2)
var grades new Object();
grades['egoing'] = 10;

// 객체의 속성에 접근하는 방법
//(1)
grades['egoing'] // 10
grades['ego' + 'ing'] // 10

//(2)
grades.egoing // 10
grades.'ego'+'ing' // 10 
```

여기서 egoing은 key가 되고, 10은 value가 된다.



## 객체와 반복작업

배열은 인덱스를 통해 for loop을 돌릴 수 있었다.  length를 통해 크기를 알 수 있음

배열은 저장된 데이터들이 순서를 가지고 있다. 먼저 들어간 것과 나간 것이 내부적으로 유지되기 때문에. 순서 자체도 중요한 정보

객체에 저장된 데이터는 순서는 없고 key와 value 만 있다. 

```javascript
var grades = { 'kor': 80, 'eng'=100, 'math': 90};
// for in 문

// 배열
var arr = ['a', 'b', 'c'];
for (var name in arr) {
    console.log(arr[name]);
}

// 객체 
for (key in grades) {
    document.write("key: " + key + "value : " + grades[key] + "<b1 />" )
}
// 반복문이 될 때마다 중괄호 안에 key라는 변수가 생성되고 value가 저장됨
```

# 모듈

## 모듈이란?

순수한 자바스크립트에서는 모듈 개념이 분명하게 존재하지는 않는다. 하지만 자바스크립트가 구동되는 호스트 환경에 따라 다양한 모듈화 방법이 제공된다. 

### 코드를 여러 개의 파일로 분리하는 것의 효과

- 자주 사용하는 코드를 파일로 만들어 필요할 때마다 재활용할 수 있다.
- 코드를 개선하면 이를 사용하고 있는 모든 애플리케이션의 동작이 개선된다.
- 코드 수정 시 필요한 로직을 빠르게 찾을 수 있다.
- 필요한 로직만을 로드해서 메모리의 낭비를 줄일 수 있다.
- 한번 다운로드된 모듈은 웹 브라우저에 의해 저장된다. 따라서 동일한 로직을 로드할 때 시간과 네트워크 트래픽을 절약할 수 있다.



> 호스트 환경: 자바스크립트가 구동되는 환경을 말한다. 보통 자바스크립트는 브라우저에서 구동되었지만, node.js가 구동되는 환경은 서버측 환경이다. Google Apps Script가 동작하는 환경은 구글 제품 위이다. 언어를 통해서 이 환경을 제어할 수 있다. 



## 모듈 사용 방법

`<script src="gretting.js></script>`: script 태그 안에 컨텐츠 대신 src 속성을 넣는다. 그러면 브라우저는 src 속성에 있는 파일을 다운로드해서 실행시킨다.



## Node.js에서의 모듈의 로드

모듈을 로드하는 방법은 호스트 환경에 따라 달라진다.

**node.circle.js (로드될 대상)**

```javascript
var PI = Math.PI;

exports.area = function (r) {
    return PI * r * r;
};

exports.circumference = function (r) {
    return 2 * PI * r;
}
```

**node.demo.js(로드의 주체)**

```javascript
var circle = require('./node.circle.js')l
console.log('The area of a circle of radius 4 is ' + circle.area(4));
```



## 라이브러리

모듈이 프로그램을 구성하는 작은 부품으로서의 로직이라면, 라이브러리는 **자주 사용되는 로직을 재사용하기 편리하도록 잘 정리한 일련의 코드들의 집합**을 말한다.

## 라이브러리의 사용

자바스크립트 라이브러리 jQuery 이용하기

```javascript
$("#execute_btn").click(fucntion(){
   $('#list li').text('coding everybody');
})
```

동일한 기능을 바닐라 자바스크립트로 구현하기

```javascript
function add(target, eventType, eventHandler, useCapture) {
    if (target.addEventListener) {
        target.addEventListener(eventType, eventHandler, useCapture?useCapture:false);
    } else if (target.attachEvent) {
        var r = target.attachEvent("on"+eventType, eventHandler);
    }
}

function clickHandler(event) {
    var nav = document.getElementById('list');
    for (var i = 0; )
}
```



# UI와 API, 문서 보는 법

##  UI와 API의 차이점

- Interface: 접점 

- UI는 User Interface의 약자로 사용자를 대면하는 접점이 되는 지점을 말한다. 
- API는 Applicationf Programming Interface의 약자로 프로그램이 동작하는 **환경을 제어**하기 위해서 환경에서 제공되는 조작 장치이다. 이 조작 장치는 **프로그래밍 언어**를 통해서 조작할 수 있다. (코드의 형태를 띄고 있는 인터페이스)  
- ex) alert이라는 명령어( 즉 api를 통해서)만 입력하면 경고창을 적은 노력을 통해서 띄울 수 있음. 경고창은 우리가 만든 것이 아니면서 만든 것이 됨...  브라우저 만드는 사람들도 운영체제가 제공해주는 api를 사용해서 만든 것이다.

- **일반인은 UI**를 통해서 시스템을 제어하고, **개발자는 API**를 통해서 시스템을 제어한다. 



## 레퍼런스와 튜토리얼

프로그래밍을 공부하기 위한 자료

- 레퍼런스(reference): 명령어의 사전
- turorial(안내서): 언어의 문법 설명



## 자바스크립트의 API

- 자바스크립트의 api
  - 자바스크립트 언어 자체의 API
    - ex) math, 정규표현식 등등 (우리 수업의 목표!)
    - 문서) ECMAScript, 생활코딩 자바스크립트 사전, 자바스크립트 레퍼런스 MDN, 
  - 자바스크립트가 동작하는 호스트 환경의 API
    - 호스트 환경: 웹 브라우저, nodejs, Google apps script
    - 문서) 웹 브라우저 API, Node.js API, Google Apps Script API

LiveScript => JavaScript => ECMAScript (표준, ECMA: 국제기구)



# 정규표현식

## 사용하는 이유

정규표현식은 일종의 언어이고, 다른 수많은 언어(자바, perl)도 정규표현식을 사용하고 있다. 텍스트 안에 어떤 정보가 있는지 찾아야 한다거나, 여러 정보들 중 패턴에 해당되는 부분을 찾아서 다른 텍스트로 치환해야할 때 필요하다. 자바스크립트는 웹, 특히 정보와 밀접한 연관을 가지고 있기 때문에 정규표현식은 중요하다.

## 사용 방법

정규표현식은 두 가지 단계로 이루어진다.

- 컴파일 단계: 패턴을 찾는 것
- 실행 단계: 찾은 패턴(대상)에 대해서 어떠한 작업을 구체적으로 하는 것

## 컴파일 단계

```javascript
// 1. 정규 표현식 리터럴 
var pattern = /a/;

// 2. 정규 표현식 객체 생성자
var pattern = new RegExp('a');
```

찾고자 하는 정보를 패턴이라는 변수에 저장했다. 

```javascript
var pattern = /a./
pattern.exec('abcdef'); // ["ab"]
```



### 정규 표현식 메서드 실행

- 정보들 중 url / 이메일/ 비밀번호 / 날짜 에 해당하는 정보를 추출할 때 정규표현식을 사용한다.
- 정규표현식으로 하는 것 
  - 필요한 정보를 **추출**
  - 확인하고자 하는 정보가 있는 지 **test**
  - 찾은 걸 다른 텍스트로 **치환**하는 것

#### RegExp.exec()

정규표현식이 찾고자 하는 문자를 첫번째 인자로 전달을 해서 패턴이 찾는 정보를 대상에서 찾아서 있다면 배열로 리턴하는 메서드이다.

```javascript
pattern.exec('abcdef'); // ["a"]
pattern.exec('bcdefg'); // null
```



#### RegExp.test()

찾고 있는 정보의 존재 여부 테스트하여 true/false 값으로 리턴하는 메서드이다.

```javascript
pattern.exec(pattern.test('abcdef')); //true
pattern.exec(pattern.test('bcdefg')); //false
```



### 문자열 메서드 실행

#### String.match()

```javascript
'abcdef'.match(pattern); // ["a"]
'bcdefg'.match(pattern); // null
```

`RegExp.exec()`와 비슷하다.



#### String.replace

````javascript
'abcdef'.replace(pattern, 'A'); // Abcdef
````



### 옵션

#### i: 대소문자 구분X

```javascript
var xi = /a/;
"Abcde".match(xi); // null

var oi = /a/i;
"Abcde".match(oi); // ["A"]
```

#### g:  검색된 모든 결과 리턴

```javascript
var xg = /a/;
"abcdea".match(xg);
var og = /a/g;
"abcdea".match(og); //["a", "a"]
```

```javascript
var oig = /a/ig;
"AabcAa".match(oig); //["A", "a", "A", "a"]
```



## 사례

### 캡쳐

괄호 안의 패턴(그룹)은 마치 변수처럼 재사용할 수 있다. 그룹을 지정하고 지정된 그룹을 가져와서 사용할 수 있는 개념을 캡쳐라고 한다. 

- `()`: group
- `\w`: 문자(A~Z, a~z, 0~9)
- `+`: 수량자(하나 이상)
- `\s`: 공백

```javascript
var pattern = /(\w+)\s(\w+)/;
var str = "coding everybody";
var result = str.replace(pattern, "$2, $1");
console.log(result); //everybody, coding
```

- `$2`: 2번째 그룹

- `$1`: 1번째 그룹 



### 치환

본문의 URL을 링크 html 태그로 교체하는 코드

``` javascript
var urlPattern = /\b(?:https?):\/\/[a-z0-9-+&@#\/%?=~_|!:,.;]*/gim;
var content = '생활코딩 : http://opentutorials.org/course/1 입니다. 네이버 : http://naver.com 입니다.';
var result = content.replace(urlPattern, function(url){
    return '<a href="'+url+'">'+url+'</a>';
});
console.log(result);
// replace라는 메서드 안에서 이 메서드가 실행될 때 urlPattern에 해당하는 텍스트를 찾을 때마다 두 번째 인자로 전달된 함수가 replace 내부에서 호출된다. 호출된 시점에서 검색된 문자열을 인자로 전달하도록 약속되어 있다. 
// original 정보는 ~로 치환된다.
// 작업이 끝나면 그것을 문자열로 리턴해줌
```



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



## 값으로서의 함수와 콜백

### 값으로서의 함수

- 자바스크립트에서는 함수도 객체이다. 다시 말해 일종의 값이다.

```javascript
// 변수 a에 담겨진 값
function a() {}

// 객체의 속성 값: 메서드
a = {
    b:function(){}
};
```



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







## 함수

### 값으로서의 함수

- 하나의 로직을 재실행할 수 있도록 하는 것으로 코드의 재사용성을 높여 준다. 

함수의 정의와 호출

```javascript
function numbering() {
    i = 0;
    while(i < 10) {
        document.write(i);
        i += 1;
    }
}
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


