---
title: ":yellow_heart: JavaScript 기본: 객체"
categories: TIL
tags: [ TIL ]
toc: true
---

# 객체

- 서로 연관되어 있는 데이터를 담아내기 위한 그릇이라는 점에서 배열과 객체는 유사하다. 

- 배열은 데이터를 담으면 인덱스가 자동으로 붙는다. 
- 객체는 인덱스의 값으로 직접 원하는 데이터를 지정할 수 있다. 

## 객체 만들기

``` javascript
var grades = {'egoing': 10, 'k8805': 6, 'sorialgi': 80}
{Key: value}

// 객체 생성 방법 (1)
var grades = {};

// 객체 생성 방법 (2)
var grades new Object();
grades['egoing'] = 10;

// 객체의 속성에 접근하는 방법 (1)
grades['egoing'] // 10
grades['ego' + 'ing'] // 10

// 객체의 속성에 접근하는 방법 (2)
grades.egoing // 10
grades.'ego'+'ing' // 10 
```

여기서 egoing은 key가 되고, 10은 value가 된다.



## 객체와 반복작업

- 배열
  - 인덱스를 통해 반복문을 돌릴 수 있었다.  
  - length를 통해 크기를 알 수 있다.
  - 저장된 데이터들이 순서를 가지고 있다. 

- 객체
  - 객체에 저장된 데이터는 순서는 없고 key와 value 만 있다. 

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


