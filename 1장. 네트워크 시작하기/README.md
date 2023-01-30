# 1장. 네트워크 시작하기
## 1. 네트워크 구성도 살펴보기
### 1. 홈 네트워크
- 인터넷 ↔ 모뎀 ↔ 공유기 ↔ 단말기 간의 연결 필요
- 무선 연결 : 무선 랜 카드, 무선 신호를 보낼 수 있는 매체(공기)
- 유선 연결 : 유선 랜 카드(이더넷 랜 카드), 랜 케이블

### 2. 데이터 센터 네트워크
- 안정적이고 빠른 대용량 서비스 제공을 목표로 구성
- 다양한 이중화 기술 보유, 높은 통신량 수용 가능(고속 이더넷 기술)
- 기존의 3계층 구조 → 2계층 구조로 변화(가상화 기술과 높은 대역폭 요구하는 스케일 아웃 기반의 서비스 등장)
- 스파인-리프 구조 : 일반 서버(10G Base-T, Top of Rack 스위치와 연결) ↔ 리프 스위치(TOR 스위치) - 스파인 스위치(40G, 100G)

## 2. 프로토콜
- 이더넷-TCP/IP 기반 프로토콜로 변경
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
