## OSI 7계층
- 통신이 일어나는 과정을 7단계로 구분하여 작성
- 데이터 흐름이 한눈에 보이며, 계층을 7단계로 구분하고 각 층별로 표준화하여 여러 회사 장비를 사용해도 네트워크에 이상이 없음.
- 상위 계층(7,6,5), 하위 계층(4,3,2,1)으로 나뉨.

### 1 계층 Physical Layer
- 물리 계층. 네트워크 통신을 위한 물리적인 표준을 정의하는 계층.
- 두 컴퓨터 간의 전기적, 기계적, 절차적인 연결을 정의하는 계층.
- 케이블 종류, 데이터 송수신 속도, 신호의 전기 전압 등.


### 2 계층 Data Link Layer
- 데이터 링크 계층. 물리적 계층을 통한 데이터 전송에 신뢰성을 제공.
- 이를 위해 물리적 주소(MAC) 지정, 네트워크 토폴로지, 오류통지, 프레임의 순차적 전송, 흐름제어 등의 기능을 가짐.
- 직접 연결되어 있지 않은 네트워크에 대해서는 상위 계층에서 오류 제어를 담당.
- 두가지 하위 계층이 존재.
1. Logical Link Control - 통신 장치간의 연결을 설정하고 관리하는 책임
2. Media Access Control - 다중 장치가 같은 미디어 채널을 공유, 제어
- 2 계층 장비로는 Switch, Bridge가 있음.

### 3 계층 Network Layer
- 네트워크 계층. Logical Address(IP, IPX)를 담당.
- 단위로 패킷(Packet)을 사용.
- 경로 선택, 라우팅, 논리적인 주소(ip)를 정의하는 계층.
- 라우팅 프로토콜을 이용하여 목적지까지 최적경로를 선택. 대표 프로토콜로는 VPN을 구성하는데 사용되는 IPSec이 있음.
- 3계층 장비로는 Router가 있음.

### 4 계층 Transport Layer
- 전송 계층. 정보를 분할하고, 상대편에 도달하기 전에 다시 합치는 과정을 담당(분할과 병합).
- 단위로는 세그먼트(Segment)를 사용.
- 목적지 컴퓨터에서 발신지 컴퓨터 간의 통신에 있어 흐름 제어, 혼잡 제어, 오류 제어를 담당.
- 전송 방식을 결정(포트 번호, TCP or UDP 등)
- 프로토콜의 종류
1. TCP: 신뢰성, 연결지향성, Connection-ful(연결을 유지하며 전송하는 방식)
2. UDP: 비신뢰성, 비연결지향성, Connection-less(연결을 유지하지 않고 전송하는 방식, Data 손실 신경안씀)

### 5 계층 Session Layer
- 세션 계층. 
- 네트워크 상에서 통신을 할 경우 양쪽 host 간 최초 연결이 되게 하고 통신 중 연결이 지속되도록 시켜주는 역할을 하는 계층.
- 통신을 하는 두 host 사이에 세션을 열고, 닫고, 관리하는 기능을 담당.
- 중요한 기능에는 동기화, 대화 기능이 있음.
- 동기란 통신 양단에서 서로 동의하는 논리적인 공통처리 지점으로, 동기점을 설정하기위해 사용됨.
- 동기점은 오류 복구를 위해 필수적으로 사용됨. 동기점이 설정된다는 것은 설정 이전까지의 통신은 서로 완벽하게 처리됐다는 것을 의미. 동기점 이전 과정은 복구가 불필요하고, 이후 처리 과정에 대한 복구 절차가 진행됨.
<br/>

- 대화는 데이터 전송 과정을 의미.
- 시간 경과에 따라 순차적으로 동기점을 부여하여 신뢰성 보장 기능을 단계적으로 구현할 수 있게 됨.
- 의도적으로 일시 정지하여 나중에 이어서 작업을 하는 것이 가능.
- 데이터 송수신 방식(Duplex), 반 이중 방식(Half Duplex), 전 이중 방식(Full Duplex)의 통신과 함께, 체크 포인팅과 유휴, 종료, 다시 시작 과정을 수행.
- 대표 프로토콜로는 SSL/TLS가 있음.
- TCP, IP, IPX 등 전송계층 프로토콜을 연결해주는 역할.
- 7,6,5 계층을 통해 데이터가 만들어짐.

### 6 계층 Presentation Layer
- 표현 계층.
- 전송하는 데이터의 Format(구성 방식)을 결정하는 계층.
- 다양한 데이터 포맷을 일관되게 상호 변환, 압축 및 암-복호화 기능을 수행.

### 7 계층 Application Layer
- 응용 계층. 사용자 인터페이스의 역할을 담당하는 계층.
- 즉, 사용자가 이용하는 네트워크 응용프로그램(ex) IE)
- 사용자와 가장 가까운 프로토콜을 정의(ex) HTTP, FTP, Telnet, SMTP, DNS, ...)


![image](https://user-images.githubusercontent.com/70579655/158925905-89833789-c38f-486d-b64f-963fe79ed985.png)


## TCP/IP 모델
### TCP 
- 전송 계층의 프로토콜.
- 연결 지향형 프로토콜.
- 세션의 연결과 종료, 흐름제어, 혼잡제어, 오류제어, 패킷의 분할 및 재조립에 사용.

### IP
- 네트워크 계층의 프로토콜
- 비연결 지향형 프로토콜. 지정한 IP 주소에 패킷이라는 통신 단위로 데이터를 전달.
- 데이터 전송에 사용. 패킷을 어디로 어떻게 처리할지를 담당.
- 비연결성이기에 패킷을 받을 대상이 없거나 받을 수 없는 상태여도 패킷을 그대로 전송.
- 비신뢰성이기에 패킷이 중간에 소실되더라도 클라이언트는 이를 파악할 수 없음. 전달하고자 하는 데이터의 용량이 클 경우 여러 패킷으로 나뉘어 데이터를 전달하게 되는데, 이때 패킷이 의도하지 않은 순서대로 서버에 도착할 수 있음.

### TCP/IP
- IP 프로토콜의 문제점을 보완하기 위해, 전송 계층에서 만든 TCP 세그먼트에 IP 패킷 정보를 감싼 TCP/IP 패킷을 만듦.
- 전송 계층에서 네트워크 계층으로 내려보낸 TCP 세그먼트에는 전송할 데이터, 출발지-목적지 포트, 전송 제어, 순서 등이 있는데, 이 세그먼트에 IP 패킷(출발-목적지 IP, ...)을 감싸 만든 TCP/IP 패킷을 만들어 기존 문제점을 보완함.
![image](https://user-images.githubusercontent.com/70579655/158925855-838fdabd-dd5d-4dbb-9b56-d36d301ec3f6.png)
