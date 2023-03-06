---
title: "[네트워크] TCP vs UDP"
excerpt: "IP 통신과 이를 보안하기 위한 TCP/UDP에 대해 알아보자"

categories:
  - Network
tags:
  - [IP, TCP, UDP]

permalink: /Network/TCP vs UDP

toc: true
toc_sticky: true

date: 2023-03-06
last_modified_at: 2023-03-06
---
<hr>

## 📝 IP &#47; IP Packet
복잡한 인터넷 망 속 수많은 노드(하나의 서버 컴퓨터)들을 지나 어떻게 클라이언트와 서버가 통신할 수 있을까? 데이터를 안전하게 전달하기 위해선 규칙이 필요하다.
<figure>
  <img src="/assets/images/posts_img/Network/ip.png">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>
 그래서 흔히 말하는 IP(인터넷 프로토콜) 주소를 컴퓨터에 부여하고 이를 이용해 통신한다.
IP는 지정한 IP 주소(IP Address)에 패킷(Packet)이라는 통신 단위로 데이터를 전달한다.

<figure>
  <img src="/assets/images/posts_img/Network/packet.png">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>
IP 패킷에서 패킷은 pack과 bucket이 합쳐진 단어로 소포에 비유할 수 있다.
IP 패킷은 우체국 송장처럼 전송 데이터를 무사히 전송하기 위해 출발지 IP, 목적지 IP와 같은 정보가 포함되어 있다.

<figure>
  <img src="/assets/images/posts_img/Network/client.png">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>
패킷 단위로 전송하면 노드들은 목적지 IP에 도달하기 위해 서로 데이터를 전달한다. 이를 통해 복잡한 인터넷 망 사이에서도 정확한 목적지로 패킷을 전송할 수 있다.

<figure>
  <img src="/assets/images/posts_img/Network/server.png">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>
서버에서 무사히 데이터를 전송받는다면 서버도 이에 대한 응답을 돌려줘야한다. 서버 역시 IP 패킷을 이용해 클라이엍느에 응답을 전달한다.

<hr class="sub">

### 🤯 IP 프로토콜 한계

정확한 출발지와 목적지를 파악할 수 있다는 점에서 인터넷 포로토콜은 적절한 통신 방법으로 보인다. 하지만, IP에는 **비연결성**과 **비신뢰성**이라는 한계가 있다.

<strong>1. 비연결성</strong>

만약 패킷을 받을 대상이 없거나 서비스 불능 상태여도 클라이언트는 서버의 상태를 파악할 방법이 없다.
그렇기 때문에 패킷을 그대로 전송하게 된다.

<strong>2. 비신뢰성</strong>

중간에 있는 서버가 데이터를 전달하던 중 장애가 생겨 패킷이 중간에 소실되더라도 클라이언트는 이를 파악할 방법이 없다.

또한, 전달 데이터의 용량이 클 경우 이를 패킷 단위로 나눠서 전달하게 되는데 이때 패킷들은 중간에 서로 다른 노드를 통해 전달될 수 있다. 이렇게 되면 클라이언트가 의도치 않은 순서로 서버에 패킷이 도착할 수 있다.

<hr>

## 📝 TCP
전송제어 프로토콜(Transmission Control Protocol)

네트워크 프로토콜 계층은 다음과 같이 나눌 수 있다. 앞서 말했던 IP프로토콜의 한계점은 보다 더 높은 계층의 TCP 프로토콜이 보완할 수 있다.
<figure>
  <img src="/assets/images/posts_img/Network/osi.png">
  <figcaption>TCP/IP 모델이 OSI 모델보다 먼저 개발되었으며 각 모델의 계층이 정확하게 일치하지는 않는다</figcaption>
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>

ex) 채팅 프로그램에서 메시지를 보낼 때
<figure>
  <img src="/assets/images/posts_img/Network/ex.png">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>
1. HTTP 메시지가 생성되면 Socket을 통해 전달된다. <br>
_*Socket(소켓) 프로그램이 네트워크에서 데이터를 송수신할 수 있도록, 네터워크 환경에 연결할 수 있게 만들어진 연결부_
2. IP 패킷을 생성하기 전 TCP 세그먼트를 생성한다.
3. 이렇게 생성된 TCP/IP 패킷은 LAN 카드와 같은 물리적 계층을 지나기 위해 이더넷 프레임 워크에 포함되어 서버로 전송된다.

<hr class="sub">

### ✔️ TCP &#47; IP 패킷
<figure>
  <img src="/assets/images/posts_img/Network/tcp.png">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>
TCP 세그먼트에는
- IP 패킷의 출발지 IP, 목적지 IP
- 정보를 보완할 수 있는 출발지 PORT, 목적지 PORT
- 전송 제어
- 순서
- 검증 정보
등을 포함한다.

<hr class="sub">

### ✔️ TCP 특징
<h4 class="sub-title">1. 연결지향 - TCP 3 way handshake</h4>

TCP는 장치들 사이에 논리적인 접속을 성립하기 위해 3 way handshake를 사용하는 연결지향형 프로토콜이다.

<figure>
  <img src="/assets/images/posts_img/Network/3way.png">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>

**■ 연결방식**
1. 클라이언트: 서버에 접속을 요청하는 SYN 패킷을 보낸다.
2. 서버: SYN요청을 받고 클라이언트에게 요청을 수락한다는 ACK와 SYN가 설정된 패킷을 발송하고 클라이언트가 당시 ACK으로 응답하기를 기다린다.
3. 클라이언트가 서버에 ACK을 보낸 이후부터 연결이 성립되며 데이터를 전송할 수 있다.

>SYN: Synchronize, ACK: Acknowledgment

**■ 만약 서버가 꺼져 있다면?**

클라이언트가 SYN을 보내고 서버에서 응답이 없기 때문에 데이터를 보내지 않는다. <br>
현재에는 최적화가 이루어져 3번 ACK을 보낼때 데이터를 함께 보내기도한다.

<h4 class="sub-title">2. 데이터 전달 보증 - 비연결성 보완</h4>
<figure>
  <img src="/assets/images/posts_img/Network/ch1.png">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>
TCP는 데이터 전송이 성공적으로 이루어진다면 이에 대한 응답을 돌려준다. ➡️ IP 패킷의 한계인 비연결성 보완

<figure>
  <img src="/assets/images/posts_img/Network/ch2.png">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>
<h4 class="sub-title">2. 순서 보장 - 비신뢰성 보완</h4>
만약 패킷이 순서대로 도착하지 않는다면 TCP 세그먼트에 있는 정보를 토대로 다시 패킷 전송을 요청할 수 있다. ➡️ IP 패킷의 한계인 비신뢰성 보완

<hr>

## 📝 UDP
사용자 데이터그램 프로토콜 (User Datagram Protocol)

UDP는 IP에 PORT, 체크섬 필드 정보만 추가된 단순한 프로토콜이다.
>*체크섬(checksum): 중복 검사의 한 형태로, 오류 정정을 통해 공간(전자 통신)이나 시간(기억 장치) 속에서 송신된 자료의 무결성을 보호하는 단순한 방법

### ✔️ UDP 특징
- 비연결지향 <br>
TCP의 3 way handshake 방식을 사용하지 않기 때문에 TCP 보다 빠른 속도를 보장한다.

- 커스터마이징 가능 <br>
HTTP3는 UDP를 사용하며 이미 여러 기능이 구현된 TCP보다는 자유롭게 커스터마이징이 가능하다.

- 데이터 전달 및 순서가 보장되진 않지만 단순하고 빠르다.
- 신뢰성보다는 연속성이 중요한 서비스(ex.실시간 스트리밍)에 자주 사용된다.

<hr>

## 📝 TCP vs UDP

|TCP|UDP|
|:--:|:--:|
|연결지향형 프로토콜|비 연결지향형 프로토콜|
|전송 순서 보장|전송 순서 보장 X|
|데이터 수신 여부 확인함|데이터 수신 여부 확인 안함|
|신뢰성 높지만 속도 느림|신뢰성 낮지만 속도 빠름|