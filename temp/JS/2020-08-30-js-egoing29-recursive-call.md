---
title: ":yellow_heart: [JS] 생활코딩: 재귀함수"
categories: TIL
tags: [ TIL ]
toc: true
---

# 재귀함수

## 패턴

- 정의: 자주 사용하는 로직의 구현 방법과 그것의 이름
- 장점
  - 문제 해결 방법 습득 용이
  - 해결 방법 논의 시 효율적인 의사소통 가능



## 재귀함수

- 함수가 자기 자신을 호출
- 

```html
<!DOCTYPE html>
<html>
    <body id ="start">
        <ul>
            <li><a href="./532">html</a></li>
            <li><a href="./533">html</a></li>
            <li><a href="./534">html</a>
            	<ul>
                	<li><a href="./535">JavaScript</a></li> 
                    <li><a href="./535">JavaScript</a></li>                   
                    <li><a href="./535">JavaScript</a></li>
                </ul>
            </li>
        </ul>
        <script>
        funcion traverse(target, callback) {
            if(target.nodeType == 1) {
                //if(target.nodeName === 'A')
                callback(target);
                var c = target.childNodes;
                for (var i = 0; i < c.length; i++) {
                    traverse(c[i], callback);
                }
            }
        }
        traverse(document.getElementById('start'), function(elem){
            console.log(elem)
        })
        </script>
    </body>
</html>
```

