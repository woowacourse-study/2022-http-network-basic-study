# HTTP 메시지 헤더란?
HTTP 프로토콜에서 리퀘스트와 리스폰스에는 반드시 메시지 헤더가 포함되어 있다. 메시지 헤더에는 클라이언트나 서버가 리퀘스트나 리스폰스를 처리하기 위한 정보가 들어 있다.

![image](https://user-images.githubusercontent.com/46641538/166202450-3c43d8c6-fe75-4b6f-a560-cfabb5358a7c.png)


# HTTP 헤더 필드
HTTP 헤더 필드는 중요한 정보를 전달한다. 메시지 바디의 크기나 사용하고 있는 언어, 인증 정보 등을 브라우저나 서버에 제공하기 위해 사용되고 있다.

# HTTP 헤더 필드의 구조
헤더 필드 명과 필드 값으로 구성되어있고 콜론으로 나뉘어져 있다.

>헤더 필드 명 : 필드 값
> 
>Content-Type:text/html


>만약 헤더 필드가 중복된 경우 어떤 브라우저는 최초의 헤더 필드를 우선적으로 처리하고 어떤 브라우저는 마지막 브라우저를 우선적으로 처리한다
> 
> 
 
# HTTP 헤더 필드의 종류
## 1. 일반적 헤더 필드
리퀘스트 메시지와 리스폰스 메시지 둘 다 사용.

메시지 전체에 적용되는 정보. ex) Connection: close

## 2. 리퀘스트 헤더 필드
클라이언트 측에서 서버 측으로 송신된 리퀘스트 메시지에 사용되는 헤더. 
쉽게 말해 요청 정보. ex) User-Agent: Mozila/5.0 (Macintosh; ..)

## 3. 리스폰스 헤더 필드
반대로 서버 측에서 클라이언트 측으로 송신한 리스폰스 메시지에 사용한다. 서버 정보, 클라이언트 추가 정보 요구 등.

ex) Server: Apache

## 4. 엔티티 헤더 필드
엔티티 바디 정보

ex) Content-Type: text/html, Content-length

[RFC 2616 헤더 필드 종류 링크](https://datatracker.ietf.org/doc/html/rfc2616#section-14.1)

>HTTP 완벽 가이드에서는 여기에 한가지를 헤더를 더 말하고 있다. 확장 헤더(Extension Header)를 더하는데 개발자들에 의해 만들어졌지만 HTTP 명세에 아직 추가되지 않은 비표준 헤더다.

# End-to-end 헤더와 Hop-by-hop 헤더
HTTP 헤더 필드는 크게 두 가지 카테고리가 있다.

### End-to-end 헤더
이 헤더는 리퀘스트나 리스폰스의 최종 수신자에게 전송된다. 캐시에 의한 리스폰스여도 다시 전송해야 한다.

### Hop-by-hop 헤더
이 분류의 헤더 중에는 한 번 전송에 대해서만 유효하고 캐시와 프록시에 의해서 전송되지 않는 것도 있다.

# HTTP/1.1 일반 헤더 필드
리퀘스트 메시지와 리스폰스 메시지 양쪽에서 사용된다.

##  Cache-Control
캐싱 동작을 지정한다.

캐시 리퀘스트 디렉티브 예시

- no-cache: 오리진 서버에서 재검증
    - 캐시된 리스폰스를 클라이언트가 받아들이지 않는다.
- no-store: 캐시된 리퀘스트, 리스폰스의 일부분을 저장해서는 안된다.
    - 리퀘스트 혹은 리스폰스에 기밀 정보가 포함된 경우
- max-age: 리스폰스 최대 age 값
    - 지정된 값보다 오래되지 않았을 경우 캐시된 값을 받는다.
...
      
캐시 리스폰스 디렉티브
- public: 어디선가 리스폰스 캐시 가능
- private: 특정 유저에 대해서만 리스폰스
- no-cache: 유효성 재확인 없이는 캐시 재사용 불가능
...

## Connection
- 프록시에게 더 이상 전송하지 않는 헤더 필드 지정
- 지속적 접근 관리

> 지속적 접근 관리?
오찌의 발표에서 지속적 접속(Persistent Connection)에 대해 다뤘다. HTTP/1.1에서 지속적 접속(Persistent Connection)이 디폴트다. 서버 측에서 명시적으로 접속을 끊고 싶을 경우 Connection 헤더 필드에 Close라고 지정한다.


### 클라이언트 리퀘스트
![image](https://user-images.githubusercontent.com/46641538/166202506-4bea1c4c-9607-481a-8c36-178b7e7771e8.png)

HTTP/1.1 이전에는 지속적 접근이 디폴트가 아니였기 때문에 오래된 버전의 HTTP에서 지속적 접근을 하고 싶으면 Connection 헤더 필드에 Keep-alive를 지정해줘야 한다.

![image](https://user-images.githubusercontent.com/46641538/166202520-ce4de462-2fba-4a92-bd7c-1b24dd23b8ea.png)

## Pragma
Pragma는 HTTP/1.0과 호환성을 위해 있는 헤더 필드다. 일반 헤더 필드지만 리퀘스트에서만 사용된다. 캐시된 리스폰스를 원하지 않음을 모든 중간 서버에 알려준다.

하지만 중간 서버의 HTTP 버전을 모르는 경우가 많기 때문에 둘 다 쓰는 경우도 있다.

>Cache-Control: no-cache
>
>Pragma: no-cache
 
## Transfer-Encoding
메시지 바디의 전송 코딩 형식을 지정.

## Upgrade
HTTP 및 다른 프로토콜의 새로운 버전이 통신에 이용되는 경우. Upgrade 헤더 필드를 사용하는 경우에는 Connection: Upgrade도 지정해줘야 한다. Upgrade 헤더 필드가 달린 리퀘스트에 대해 서버는 상태 코드 101 Switching Protocols로 리스폰스 할 수 있다.

## Via
리퀘스트 혹은 리스폰스 메시지의 경로를 알기 위해 사용된다.
프록시 서버들을 경유하면서 Via 필드에 점점 프록시 서버들의 정보가 추가된다.

# Warning
HTTP/1.0에서는 리스폰스 헤드가 변경된 것이다. 리스폰스에 관한 추가 정보를 전달해준다. 기본적으로 캐시에 관한 문제의 경고를 유저에게 전달한다.
