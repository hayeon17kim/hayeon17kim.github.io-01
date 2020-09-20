## HTTP 메시지

HTTP 메시지는 서버와 클라이언트 간에 **데이터가 교환되는 방식**이다. 메시지의 타입은 요청(request)와 응답(response)로 나뉘어진다.



## 메시지 특징

- ASCII로 인코딩된 텍스트 정보이며 여러 줄로 되어 있다.
- HTTP 초기와 HTTP1.1의 메시지는 사람이 읽을 수 있었지만, HTTP/2에서는 최적화와 성능 향상을 위해 HTTP 프레임으로 나누어졌다.
- HTTP 메시지는 개발자 대신 소프트웨어, 브라우저, 프록시 혹은 웹 서버가 대신 작성해준다.  프록시, 서버의 경우 설정 파일을 통해, 브라우저의 경우 API 혹은 다른 인터페이스를 통해 제공된다.



HTTP/2의 binary framing 매커니즘은 사용 중인 API나 설정 파일을 변경하지 않아도 되도록 설계되었기 때문에 사용자가 이해하기 쉽다.



## HTTP 요청과 응답의 구조

![HTTPMessageStructure](https://mdn.mozillademos.org/files/13827/HTTPMsgStructure2.png)

- **start-line**: 실행되어야 할 요청, 요청 수행에 대한 결과
- **option**: HTTP 헤더 세트: 요청에 대한 설명, 메시지 본문에 대한 설명
- 빈 줄(blank line): 요청에 대한 모든 메타 정보가 전송되었음을 알린다.
- 요청과 관련된 내용, 응답과 관련된 문서 등이 옵션으로 들어간다. 
  - 본문의 존재 유무 및 크기는 HTTP 헤더에 명시된다.



## HTTP Request 구조

### start line

- HTTP 메서드
- 요청 타겟
- HTTP 버전



### Header

- General Header: 메시지 전체에 적용
- Request Header
- Entity Header



### Body

- 단일-리소스 본문(single-resource bodies)
- 다중-리소스 본문(multiple-resource bodies)



## HTTP 응답

### status line

- 프로토콜 버전
- 상태 코드
- 상태 텍스트



### Header

- General Header
- Response Header
- Entity Header



### Body

- 단일-리소스 본문(single-resource bodies)
  - 길이를 아는 단일 파일로 구성
  - 길이를 모르는 단일 파일로 구성

- 다중-리소스 본문(multiple-resource bodies)



## HTTP/2 프레임

### HTTP/1.x 메시지의 성능상의 결함

- 본문과 달리 header는 압축이 되지 않는다.
- 연속된 메시지는 비슷한 header 구조를 띄지만, 메시지마다 반복되어 전송된다.
- 다중 전송(multiplexing)이 불가능
  - 비효율적인 codl TCP 연결 사용: 서버 하나에 연결을 여러 개 열어야 한다.



### HTTP/2의 구조

![http2BinaryFraming](https://mdn.mozillademos.org/files/13819/Binary_framing2.png)

HTTP/1.x 메시지를 프레임으로 나누어 스트림에 끼워 넣는다.

데이터와 헤더 프레임이 분리되었기 때문에 헤더를 압축할 수 있다.

스트림 여러개를 하나로 묶을 수 있어서(멀티 플렉싱), 기저에 수행되는 TCP 연결이 좀 더 효율적이게 이루어진다.





모바일에서 사용하는 게 좋다. 서버에서 설정을 안하면 아예 안됨. 모바일 구동환경이 2에 특화하 되어 잇다. 서버에서 





>  time interval

>  h2db 

오름차순으로 정렬. 