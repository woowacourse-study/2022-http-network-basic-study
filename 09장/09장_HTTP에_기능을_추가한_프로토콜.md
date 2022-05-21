## SPDY
- SPDY는 'speedy'라는 단어를 기반으로 Google이 만든 용어
- Google이 자신들의 'Make the Web Faster' 노력의 하나로 제안한 새로운 프로토콜

### HTTP/1.1 의 문제점
- 요청/응답 구조에 따른 비효율성
- 브라우저 요청없이는 어떠한 데이터도 전송할 수 없는 구조
- 헤더의 비대화로 인한 트래픽 과도화

### 새로운 프로토콜이 나오게 된 이유
- 훨씬 더 많은 리소스로 구성
- 다수의 도메인을 사용
- 과거에 비해 매우 동적으로 동작
- 보안이 보다 중요한 이슈가 됨

### SPDY 특징

![img.png](img.png)

__SPDY는 특히 전송 지연(latency) 문제의 해결에 주로 집중__

- 항상 TLS(Transport Layer Security) 위에서 동작
  - https 로 작성된 웹사이트에서만 동작
- HTTP 헤더 압축
- 바이너리 프로토콜
  - 프레임을 텍스트가 아닌 바이너리로 구성하여 파싱이 더 빠르고, 오류 발생 가능성이 낮음
- Multiplexing
  - 하나의 커넥션 안에서 다수의 독립적인 스트림을 동시에 처리
- Server Push
  - 클라이언트의 요청이 없어도 서버에서 콘텐츠를 직접 push

## websocket

### 웹소켓 특징

- 웹소켓은 실시간 양방향 통신을 지원
- 한번 연결이 수립되면 클라이언트와 서버 모두 자유롭게 데이터를 보낼 수 있음
- ws, wss
- HTTP를 이용하여 연결을 수립
- 연결 된 이후에도 통신을 할 때는 사용했던 포트인 80, 443을 이용
- 핸드쉐이크가 끝나면 HTTP 프로토콜을 웹소켓 프로토콜로 변환하여 통신을 하는 구조

## 웹소켓 http 요청

```http request
GET /chat HTTP/1.1
Host: server.example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: x3JJHMbDL1EzLkh9GBhXDw==
Sec-WebSocket-Protocol: chat, superchat
Sec-WebSocket-Version: 13
Origin: http://example.com
```

- `Sec-WebSocket-Key`: 핸드쉐이크 응답을 검증할 키 값
- `Connection`: Upgrade
    - `keep-alive` 와 다른점은 연결을 keep alive 하면서 non-HTTP 요청으로 사용한다는 것
- `Upgrade`: 다른 프로토콜로 연결한다는 헤더(같은 전송 프로토콜 내에서)

```http request
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: HSmrc0sMlYUkAGmm5OPpG2HaGWk=
Sec-WebSocket-Protocol: chat
```

- `Sec-WebSocket-Accept`: 정상적인 핸드쉐이크 과정을 검증
- 업그레이드가 되면 HTTP는 사용되지 않고, 웹 소켓 연결이 닫힐 때까지 두 끝 점에서 웹 소켓 프로토콜을 사용하여 데이터를 주고 받음
- 웹 소켓 연결은 주로 새로고침이나 창 닫기 등의 이벤트 발생 시 닫힘

## HTTP/2.0

### HTTP/1.1 의 한계

- HTTP/1.1는 기본적으로 Connection당 하나의 요청을 처리 하도록 설계
- 동시전송이 불가능하고 요청과 응답이 순차적

### HTTP/2.0 특징

- Multiplexed Streams
  - 한 커넥션으로 동시에 여러개의 메세지를 주고 받음
  - 응답은 순서에 상관없이 stream으로 주고 받음
- Server Push
  - 서버는 클라이언트의 요청에 대해 요청하지도 않은 리소스를 마음대로 보내줄 수 있음
- Header Compression
  - Header 정보를 압축
  - RFC7531

## WebDAV

- HTTP 확장판
- 웹을 읽고 쓰기가 가능한 매체로 만듦
- 사용자가 서버의 문서를 만들고 변경하고 이동할 수 있는 기반 제공
- 웹 서버에 저장된 문서와 파일을 편집하고 관리하는 사용자들 사이에 협업을 손쉽게 함
- 주요 포트: 80, 443
