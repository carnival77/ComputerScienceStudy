<div align="center">
  <br />
  <h1> TCP 3-way Handshake</h1>
  <br />
</div>

## 목차

1. [**TCP 3-way Handshake 정의와 역할**](#1)
2. [**TCP 3-way Handshake 과정**](#2)
3. [**TCP 4-way Handshake 과정**](#3)

<br />

<div id="1"></div>

## TCP 3-way Handshake 의 정의와 역할

**정의**<br /><br />
TCP/IP 프로토콜을 이용해서 통신을 하는 응용프로그램이 데이터를 전송하기 전에 먼저 **정확한 전송을 보장하기 위해** 상대방 컴퓨터와 사전에 세션을 수립하는 과정을 의미한다. 
<br/><br />
**역할**
1. 양쪽 모두 데이터를 전송할 준비가 되었다는 것을 보장하고, 실제로 전달이 시작되기 전에 한쪽이 다른 쪽이 준비되었다는 것을 알수 있도록 한다. 
2. 양쪽 모두 상대편에 대한 초기 순차 일련번호를 얻을 수 있도록 한다.
<br />

<div id="2"></div>

## TCP 3-way Handshake 과정


![enter image description here](https://t1.daumcdn.net/cfile/tistory/99FB2A3D5B32ED7B0B)

(SYN 은 synchronize sequence numbers ACK는 acknowledgements 의 약자)

1. 클라이언트(A)는 서버(B)에 접속을 요청하는 SYN 패킷을 보낸다. 이때 클라이언트(A)는 SYN을 보내고 SYN/ACK 응답을 기다리는 SYN_SENT 상태가 된다.
2. 이때 서버(B)는 Listen 상태로 포트 서비스가 가능한 상태여야 한다. (Closed :닫힌상태) 서버(B)는 SYN 요청을 받고 클라이언트(A)에게 요청을 수락한다는 ACK와 SYN flag가 설정된 패킷을 발송하고 클라이언트(A)가 다시 ACK으로 응답하기를 기다린다. 이때 서버(B)는 SYN_RECEIVED 상태가 된다.
3. 클라이언트(A)는 서버(B)에게 ACK을 보내고 이후로부터는 연결이 이루어지고 데이터가 오가게 된다. 이때의 서버(B) 상태가 성립(ESTABLISHED) 이다.

위와 같은 방식으로 통신하는것이 신뢰성 있는 연결을 맺어 준다는 TCP의 3 Way handshake 방식이다.

 
| 상태 | 설명 |
|--|--|
| Closed  | 닫힌 상태  |
| Listen  | 포트가 열린 상태로 연결 요청 대기중인 상태  |
| SYN-send| SYN 요청을 한 상태  |
| SYN-Received  | SYN 요청을 받고 상대방의 응답을 기다리는 상태  |
| ESTABLISHED | 연결이 확인된 상태  |


<br />

<div id="3"></div>

## TCP 4-way Handshaking 과정
**정의** <br /><br />
세션을 종료하기 위해 수행되는 과정 

![enter image description here](https://t1.daumcdn.net/cfile/tistory/2152353F52F1C02835)

1. FIN (+ACK) 서버와 클라이언트가 TCP 연결이 되어있는 상태에서 클라이언트가 접속을 끊기 위해 close( )를 호출한다. 클라이언트가 close( )를 호출하면서 서버에게 FIN 패킷을 보내게 되고 클라이언트는 FIN_WAIT_1 상태로 들어간다.
2. ACK 서버는 클라이언트에게 응답을 보내고 CLOSE_WAIT 상태에 들어간다. 그리고 아직 남은 데이터가 있다면 마저 전송을 마친 후에 close( )를 호출한다.
3. FIN_WAIT_2 클라이언트에서는 서버에서 ACK를 받은 후에 서버가 남은 데이터 처리를 끝내고 FIN 패킷을 보낼 때까지 기다리게 됩니다.
4. FIN 서버는 이제 모든 데이터 처리가 끝났다고 종료에 합의 한다는 뜻으로 클라이언트에게 FIN 패킷을 보낸 후에 승인 번호를 보내줄 때까지 기다리는 LAST_ACK 상태로 들어간다.
5. ACK 클라이언트는 서버에서 FIN 패킷을 받고 나서 다시 서버에게 ACK 응답을 보낸 후에 TIME_WAIT 상태로 들어가며 실질적인 종료 과정(CLOSED)에 들어가게 된다. 이때 TIME_WAIT 상태는 **의도치 않은 에러로 인해 연결이 데드락으로 빠지는 것을 방지**하는데, 만약 에러로 인해 종료가 지연되다가 타임이 초과되면 CLOSED로 들어간다.
6. CLOSED 서버는 ACK를 받고 CLOSED 상태로 들어가 종료하게 된다.
<br />

**💡 TIME_WAIT 이란?**<br />
<br />
서버가 FIN을 전송하기 전에 전송한 ACK 패킷이 Routing 지연이나 패킷 유실로 인한 재전송으로 FIN 패킷보다 늦게 도착 할 수 있다. 이 경우 **데이터가 유실**되기 때문에 이를 **대비**하여 클라이언트는 서버로부터 FIN을 수신하더라도 **일정시간(2MSL time out = Maximum Segment Lifetime의 2배) 동안 세션을 남겨 놓고 잉여 패킷을 기다리는 과정**을 말한다.  
  
  
