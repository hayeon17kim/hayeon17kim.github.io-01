---
title: ":yellow_heart: JavaScript 기본: 정규표현식"
categories: TIL
tags: [ TIL ]
toc: true
---

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




