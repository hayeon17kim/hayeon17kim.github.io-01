---
title: ":yellow_heart: JavaScript 기본: 모듈"
categories: TIL
tags: [ TIL ]
toc: true
---

# 모듈

## 모듈이란?

순수한 자바스크립트에서는 모듈 개념이 분명하게 존재하지는 않는다. 하지만 자바스크립트가 구동되는 호스트 환경에 따라 다양한 모듈화 방법이 제공된다. 

## 모듈화의 효과

- 자주 사용하는 코드를 파일로 만들어 필요할 때마다 재활용할 수 있다.
- 코드를 개선하면 이를 사용하고 있는 모든 애플리케이션의 동작이 개선된다.
- 코드 수정 시 필요한 로직을 빠르게 찾을 수 있다.
- 필요한 로직만을 로드해서 메모리의 낭비를 줄일 수 있다.
- 한번 다운로드된 모듈은 웹 브라우저에 의해 저장된다. 따라서 동일한 로직을 로드할 때 시간과 네트워크 트래픽을 절약할 수 있다.

> 호스트 환경: 자바스크립트가 구동되는 환경을 말한다. 보통 자바스크립트는 브라우저에서 구동되었지만, node.js가 구동되는 환경은 서버측 환경이다. Google Apps Script가 동작하는 환경은 구글 제품 위이다. 언어를 통해서 이 환경을 제어할 수 있다. 


## 모듈 사용 방법

`<script src="gretting.js></script>`: script 태그 안에 컨텐츠 대신 src 속성을 넣는다. 그러면 브라우저는 src 속성에 있는 파일을 다운로드해서 실행시킨다.

### Node.js에서의 모듈의 로드

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

### 라이브러리의 사용

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


