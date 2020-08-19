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



## 배열





## 객체

서로 연관되어 있는 데이터를 담아내기 위한 그릇이라는 점에서 배열과 객체는 유사하다. 

배열은 데이터를 담으면 0, 1, 2라는 인덱스가 자동으로 붙는다. 반면 객체는 인덱스의 값으로 직접 원하는 데이터를 지정할 수 있다. 

### 객체 만들기

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



### 객체와 반복작업

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





# 정규표현식

## 정규표현식이란?
자바스크립트의 기능 중 하나가 정규표현식을 사용하는 조작방법. 정규표현식은 일종의 언어이고, 다른 수많은 언어(자바, perl)도 정규표현식을 사용하고 있다.
## 사용하는 이유
텍스트 안에 어떤 정보가 있는지 찾아야 한다거나, 여러 정보들 중 패턴에 해당되는 부분을 찾아서 다른 텍스트로 치환해야할 때 필요하다.
## 사용 방법
- 컴파일 단계: 패턴을 찾는 것
- 실행 단계: 찾은 패턴(대상)에 대해서 어떠한 작업을 구체적으로 하는 것

## 컴파일 단계
### 패턴 만들기
정규표현식 객체를 패턴에 저장
```javascript
// 1. 정규 표현식 리터럴 
var pattern = /a/;

// 2. 정규 표현식 객체 생성자
var pattern = new RegExp('a');
```
찾고자 하는 정보를 패턴이라는 변수에 저장했다. 

### 정규 표현식 메서도 실행 

# 클로저

- JavaScript의 함수와 다른 언어의 함수의 차이점: 함수가 값이 될 수 있다.
- 함수는 객체의 값으로 포함될 수 있다: 이처럼 객체의 속성 값으로 담겨진 함수를 메서드(method)라고 부른다

```javascript
a = {
    b: function(){}
}
```

- 다른 함수의 인자로 전달될 수 있다

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





# 클로저

## 내부함수

- 내부함수는 외부함수의 지역변수에 접근할 수 있다.



## 클로저

- 클로저란 내부함수가 외부함수의 지역변수에 접근할 수 있고, 외부함수는 외부함수의 지역변ㄷ수를 사용하는 내부함수가 소멸될 때까지 소멸하지 않는 특성을 의미한다.
- 내부 함수가 외부 함수의 맥락(context)에 접근할 수 있는 것을 가리킨다. 외부함수의 실행이 끝나서 외부함수가 소멸된 이후에도 내부함수가 외부함수에 접근할 수 있다.
- 









- 클로저는 내부 함수가 외부 함수의 맥락에 접근할 수 있는 것을 가리킨다.

- 컨텍스트란? 함수가 실행이 될 때 실행에 필요한 정보들이 저장되는 스택(컨텍스트)가 생성되고, 쌓인다. 각 컨텍스트마다 리스트 형식으로 자신이 실행되었던 환경에 대한 정보를 모두 저장하고 있다.

- 실행 컨텍스트가 생성되는 경우

  - 전역 실행 컨텍스트
  - eval() 함수로 실행되는 코드
  - 함수 안의 코드를 실행할 경우

- 의미: 클로저(함수) 스코프 안에서 클로저(함수)를 만든 외부 스코프에 항상 접근할 수 있다.
- 자바스크립트에서 함수는 실행 컨텍스트를 가지고 이 실행 컨텍스트(스포프)는 외부 함수에 대한 실행 컨텍스트에 대한 참조값도 가지고 있으므로, 외부 함수가 종료되어 스코프에 접근하지 못할 것 같은 상황에서도 외부 함수 프로퍼티에 접근할 수 있다는 개념 
- 함수가 생성될 때 환경을 그대로 유지해서 실행할 시점에 활용해야 할 때 유용하게 사용 

  



# 함수

## 값으로서의 함수

JavaScript에서 함수는 값이 될 수 있다.

- 객체의 값으로 포함될 수 있다
- 다른 함수의 인자로 전달될 수 있다.
- 함수의 리턴값으로 사용될 수 있다.
- 배열의 값으로도 사용할 수 있다.

## 콜백

### 처리의 위임

함수의 인자로 함수를 전달할 때, 이 함수는 호출될 수 있기 때문에 이를 이용하면 함수의 동작을 완전히 바꿀 수 있다.

```javascript
function sortNumber(a,b){
    // 위의 예제와 비교해서 a와 b의 순서를 바꾸면 정렬순서가 반대가 된다.
    return b-a;
}
var numbers = [20, 10, 9,8,7,6,5,4,3,2,1];
alert(numbers.sort(sortNumber)); // array, [20,10,9,8,7,6,5,4,3,2,1]
```

### 비동기 처리

