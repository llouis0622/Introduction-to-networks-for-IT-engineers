# 1장. 네트워크 시작하기

# 1. 네트워크 구성도 살펴보기

## 1. 홈 네트워크

- 인터넷 ↔ 모뎀 ↔ 공유기 ↔ 단말기 간의 연결 필요
- 무선 연결 : 무선 랜 카드, 무선 신호를 보낼 수 있는 매체(공기)
- 유선 연결 : 유선 랜 카드(이더넷 랜 카드), 랜 케이블

## 2. 데이터 센터 네트워크

- 안정적이고 빠른 대용량 서비스 제공을 목표로 구성
- 다양한 이중화 기술 보유, 높은 통신량 수용 가능(고속 이더넷 기술)
- 기존의 3계층 구조 → 2계층 구조로 변화(가상화 기술과 높은 대역폭 요구하는 스케일 아웃 기반의 서비스 등장)
- 스파인-리프 구조 : 일반 서버(10G Base-T, Top of Rack 스위치와 연결) ↔ 리프 스위치(TOR 스위치) - 스파인 스위치(40G, 100G)

# 2. 프로토콜

- 이더넷 - TCP/IP 기반 프로토콜로 변경
- 물리적 측면 : 데이터 전송 매체, 신호 규약, 회선 규격 → 이더넷
- 논리적 측면 : 장치들끼리 통신하기 위한 프로토콜 규격 → TCP/IP

```html
GET /api HTTP/1.1
Accept: text/html, application/xhtml+xml, */*
Referer: http://zigispace.net/
Accept-Language: ko-KR
User-Agent: Mozilar/5.0 (Windows NT 6.1; WOW64; Trident/7.0; TCO_2018006113830;
rv:11.0) like Gecko
Accept-Encoding: gzip, deflate
Host: theplmingspace.tistory.com
DNT: 1
Connection: Keep-Alive
```

- TCP/IP 프로토콜 스택 : 애플리케이션 계층, 트랜스포트 계층, 네트워크 계층, 데이터링크 계층, 피지컬 계층

# 3. OSI 7계층과 TCP/IP

## 1. OSI 7계층

- OSI 레퍼런스 모델
    
    
    | 애플리케이션 계층 | Data |
    | --- | --- |
    | 프레젠테이션 계층 | Data |
    | 세션 계층 | Data |
    | 트랜스포트 계층 | Segments |
    | 네트워크 계층 | Packets |
    | 데이터 링크 계층 | Frames |
    | 피지컬 계층 | Bits |
- 계층별로 프로토콜을 개발해 네트워크 구성 요소들을 모듈화 가능
- 하위 계층 : 1 ~ 4계층, 데이터 플로 계층
- 상위 계층 : 5 ~ 7계층, 애플리케이션 계층

| 계층 | 기본 용어 | 표기법 | 실무 |  |
| --- | --- | --- | --- | --- |
| 7계층 | Application Layer | 애플리케이션 계층 |  | L7 |
| 6계층 | Presentation Layer | 프레젠테이션 계층 | 표현 계층 | L6 |
| 5계층 | Session Layer | 세션 계층 |  | L5 |
| 4계층 | Transport Layer | 트랜스포트 계층 | 전송 계층 | L4 |
| 3계층 | Network Layer | 네트워크 계층 |  | L3 |
| 2계층 | Data Link Layer | 데이터 링크 계층 |  | L2 |
| 1계층 | Physical Layer | 피지컬 계층 | 물리 계층 | L1 |
- 애플리케이션 개발자 : 하향식 형식으로 개발(7 → 1)
- 네트워크 엔지니어 : 상향식 형식으로 개발(1 → 7)

## 2. TCP/IP 프로토콜 스택

- 4계층으로 이루어짐 : 애플리케이션 계층, 트랜스포트 계층, 인터넷, 네트워크 액세스
- 애플리케이션 계층 : OSI 모델의 애플리케이션 계층, 프레젠테이션 계층, 세션 계층
- 트랜스포트 계층 : OSI 모델의 트랜스포트 계층
- 인터넷 : OSI 모델의 네트워크 계층
- 네트워크 액세스 : OSI 모델의 데이터 링크 계층, 피지컬 계층

# 4. OSI 7계층별 이해하기

## 1. 1계층(피지컬 계층)

- 물리적 연결과 관련된 정보 정의(주로 전기 신호 전달)
- 1계층 장비에 전기 신호가 들어옴 → 전기 신호를 재생성하여 내보냄(모든 포트에 같은 전기 신호 전송)
- 허브(Hub), 리피터(Repeater), 케이블(Cable), 커넥터(Connector), 트랜시버(Tranceiver), 탭(TAP)
- 허브, 리피터 : 네트워크 통신을 중재
- 케이블, 커넥터 : 케이블 본체를 구성
- 트랜시버 : 컴퓨터의 랜카드와 케이블을 연결
- 탭 : 네트워크 모니터링과 패킷 분석을 위해 전기 신호를 다른 장비로 복제

## 2. 2계층(데이터 링크 계층)

- 전기 신호를 모아 우리가 알아볼 수 있는 데이터 형태로 처리
- 주소 정보를 정의하고 정확한 주소로 통신이 되도록 함
- 출발지와 도착지 주소를 확인한 후 데이터 처리 수행
- 데이터에 대한 에러 탐지, 고치는 역할 수행
- 플로 컨트롤(Flow Control) : 무작정 데이터를 전달하는 것이 아니라 받는 사람이 현재 데이터를 받을 수 있는지 확인
- 네트워크 인터페이스 카드, 스위치(Switch)
    - 네트워크 인터페이스 카드 : 네트워크 인터페이스 컨트롤러(Network Interface Controller, NIC), 네트워크 카드, 랜 카드(Lan Card), 물리 네트워크 인터페이스, 이더넷 카드(Ethernet Card), 네트워크 어댑터(Network Adapter)
- MAC 주소 체계

## 3. 3계층(네트워크 계층)

- IP 주소와 같은 논리적인 주소 정의
    - 사용자가 환경에 맞게 변경해 사용할 수 있고 네트워크 주소 부분과 호스트 주소 부분으로 나뉨
    - 172.31.0.1(앞의 두 부분 : 네트워크 주소 부분, 뒤의 두 부분 : 호스트 주소 부분)
- 라우터 : IP 주소를 이해할 수 있음, 최적의 경로를 찾아주고 해당 경로로 패킷을 전송하는 역할

## 4. 4계층(트랜스포트 계층)

- 실제로 해당 데이터들이 정상적으로 잘 보내지도록 확인
- 패킷이 유실되거나 순서가 바뀌었을 때 바로잡아 주는 역할
- 시퀀스 번호(Sequence Number) : 패킷에 보내는 순서를 명시한 것
- ACK 번호(Acknowledgement Number) : 받는 순서를 나타낸 것
- 로드 밸런서, 방화벽
    - 애플리케이션 구분자(포트 번호), 시퀀스, ACK 번호 정보를 이용해 부하를 분산하거나 보안 정책을 수립해 패킷을 통과, 차단하는 기능 수행

## 5. 5계층(세션 계층)

- 세션을 관리하는 것이 주요 역할
- 양 끝단의 응용 프로세스가 연결을 성립하도록 도와줌
- 연결이 안정적으로 유지되도록 관리
- 작업 완료 후에는 이 연결을 끊는 역할
- 에러로 중단된 통신에 대한 에러 복구와 재전송 수행

## 6. 6계층(프레젠테이션 계층)

- 표현 방식이 다른 애플리케이션이나 시스템 간의 통신을 돕기 위해 하나의 통일된 구문 형식으로 변환시키는 기능
- MIME 인코딩, 암호화, 압축, 코드 변환

## 7. 7계층(애플리케이션 계층)

- 애플리케이션 프로세스를 정의하고 애플리케이션 서비스 수행
- 네트워크 소프트웨어의 UI 부분이나 사용자 입, 출력 부분 정의
- FTP, SMTP, HTTP, TELNET

## 💭. 계층별 주요 프로토콜 및 장비

| 계층 | 주요 프로토콜 | 장비 |
| --- | --- | --- |
| 애플리케이션 계층 | HTTP, SMP, SMTP, STUN, TFTP, TELNET | ADC, NGFW, WAF |
| 프레젠테이션 계층 | TLS, AFP, SSH |  |
| 세션 계층 | L2TP, PPTP, NFS, PRC, RTCP, SIP, SSH |  |
| 트랜스포트 계층 | TCP, UDP, SCTP, DCCP, AH, AEP | 로드 밸런서, 방화벽 |
| 네트워크 계층 | ARP, IPv4, IPv6, NAT, IPSec, VRRP, 라우팅 프로토콜 | 라우터, L3 스위치 |
| 데이터 링크 계층 | IEEE 802.2, FDDI | 스위치, 브릿지, 네트워크 카드 |
| 피지컬 계층 | RS-232, RS-449, V.35, S 등의 케이블 | 케이블, 허브, 탭(TAP) |

# 5. 인캡슐레이션과 디캡슐레이션

## 1. 인캡슐레이션(Encapsulation)

- 상위 계층에서 하위 계층으로 데이터를 보내는 과정
- 패킷에 데이터를 넣을 수 있도록 분할
- 헤더에 반드시 포함되어야 하는 정보 : 현재 계층에서 정의하는 정보, 상위 프로토콜 지시자
    - 상위 프로토콜 지시자 : 프로토콜 번호(3계층)
        
        
        | 프로토콜 번호 | 프로토콜 |
        | --- | --- |
        | 1 | ICMP(Internet Control Message) |
        | 2 | IGMP(Internet Group Management) |
        | 6 | TCP(Transmission Control) |
        | 17 | UDP(User Datagram) |
        | 50 | ESP(Encap Security Payload) |
        | 51 | AH(Authentication Header) |
        | 58 | IPv6용 ICMP |
        | 133 | FC(Fibre Channel) |
    - 상위 프로토콜 지시자 : 포트 번호(4계층)
        
        
        | 포트 번호 | 서비스 |
        | --- | --- |
        | TCP 20, 21 | FTP(File Transfer Protocol) |
        | TCP 22 | SSH(Secure Shell) |
        | TCP 23 | TELNET(Telnet Terminal) |
        | TCP 25 | SMTP(Simple Mail Transport Protocol) |
        | TCP 49 | TACACS |
        | TCP 53 / UDP 53 | DNS(Domain Name Service) |
        | UDP 67, 68 | BOOTP(Bootstrap Protocol) |
        | TCP 80 / UDP 80 | HTTP(HyperText Transfer Protocol) |
        | UDP 123 | NTP(Network Time Protocol) |
        | UDP 161, 162 | SNMP(Simple Network Management Protocol) |
        | TCP 443 | HTTPS |
        | TCP 445 / UDP 445 | Microsoft-DS |
    - 상위 프로토콜 지시자 : 이더 타입(2계층)
        
        
        | 이더 타입(Ether Type) | 프로토콜 |
        | --- | --- |
        | 0x0800 | IPv4(Internet Protocol version 4) |
        | 0x0806 | ARP(Address Resolution Protocol) |
        | 0x22F3 | IETF TRILL Protocol) |
        | 0x8035 | RARP(Reverse ARP) |
        | 0x8100 | VLAN-tagged frame(802.1Q) |
        | Shortest Path Bridding(802.1aq) | AH(Authentication Header) |
        | 0x86DD | IPv6(Internet Protocol version 6) |
        | 0x88CC | LLDP(Link Layer Discovery Protocol) |
        | 0x8906 | FCoE(Fibre Channel over Ethernet) |
        | 0x8915 | RoCE(RDMA over Converged Ethernet) |
    

## 2. 디캡슐레이션(Decapsulation)

- 하위 계층에서 상위 계층의 데이터를 받는 과정
- 받은 전기 신호를 데이터 형태로 만들어 위 계층으로 올림

## 💭. MSS & MTU(데이터 크기 조절)

- MSS(Maximum Segment Size) : 네트워크에서 수용할 수 있는 크기를 역산정해 데이터가 4계층으로 내려올 때 적절한 크기로 쪼개질 수 있도록 유도
- MTU(Maximum Transmission Unit) : 네트워크에서 한 번에 보낼 수 있는 데이터 크기(1500 Byte)
- IP 헤더(20 바이트), TCP 헤더(20 바이트), MSS (1460 바이트)
