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


