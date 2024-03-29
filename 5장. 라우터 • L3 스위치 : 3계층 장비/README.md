# 5장. 라우터 / L3 스위치 : 3계층 장비

# 1. 라우터의 동작 방식과 역할

- 들어온 패킷의 목적지 주소가 라우팅 테이블에 없으면 패킷을 버림
- 패킷 포워딩 과정에서 기존 2계층 헤더 정보를 제거한 후 새로운 2계층 헤더 제작

## 1. 경로 지정

- 경로 정보를 모아 라우팅 테이블 제작
- 패킷이 라우터로 들어오면 패킷의 도착지 IP 주소 확인 → 경로 지정, 패킷 포워딩
- 경로 정보를 얻는 역할
    - IP 주소를 입력하면서 자연스럽게 인접 네트워크 정보를 얻는 방법
    - 관리자가 직접 경로 정보를 입력하는 방법
    - 라우터끼리 서로 경로 정보를 자동으로 교환하는 방법
- 얻은 경로 정보를 확인, 패킷 포워딩 역할

## 2. 브로드캐스트 컨트롤(Broadcast Control)

- 분명한 도착지 정보가 있을 때만 통신 허가
- 바로 연결되어 있는 네트워크 정보를 제외하고 경로 습득 미설정 시 패킷 포워딩 불가능
- 브로드캐스트가 다른 네트워크로 전파되는 것 방지

## 3. 프로토콜 변환

- 서로 다른 프로토콜로 구성된 네트워크 연결
- WAN 구간은 PPP와 같은 WAN 프로토콜 사용
- LAN 구간은 이더넷 프로토콜 사용

# 2. 경로 지정 - 라우팅 / 스위치

- 경로 정보를 얻어 경로 정보 정리
- 정리된 경로 정보를 기반으로 패킷 포워딩
- 패킷이 들어오기 전에 경로 정보를 충분히 수집하고 있어야 라우터가 정상적으로 동작
- 서머리 작업 : 서브넷 단위로 라우팅 정보 습득, 라우팅 정보 최적화 → 여러 개의 서브넷 정보를 뭉쳐 전달
- 목적지에 가장 근접한 정보를 찾아 패킷 포워딩

## 1. 라우팅 동작과 라우팅 테이블

- 홉-바이-홉(Hop-by-Hop) : 네트워크를 한 단계씩 뛰어넘음
- 넥스트 홉(Next Hop) : 인접한 라우터
    - 다음 라우터의 IP 지정(넥스트 홉 IP 주소)
    - 라우터의 나가는 인터페이스 지정
    - 라우터의 나가는 인터페이스와 다음 라우터의 IP 동시 지정
- 최적의 넥스트 홉 선택 후 보냄
- 라우터에서 넥스트 홉 지정 : 상대방 라우터의 인터페이스 IP 주소를 지정
- 특수한 경우 : 상대방 넥스트 홉 라우터의 IP를 모르더라도 MAC 주소 정보를 알아낼 수 있을 때만 사용
- 라우팅 테이블 제작 : 목적지 정보만 수집하고 패킷이 들어오면 목적지 주소를 확인해 패킷을 넥스트 홉으로 포워딩
    - 목적지 주소
    - 넥스트 홉 IP 주소, 나가는 로컬 인터페이스(선택 가능)
    - PBR(Policy-Based Routing), 소스 라우팅(Source Routing), 폴리시 라우팅(Policy Routing)

## 💭. 소스 라우팅과 PBR, 정책 라우팅

- PBR을 출발지 주소로 이용해 경로 지정
- 라우터가 경로를 지정하는 것이 아니라 출발지에서 경로를 지정하는 것

## 💭. 루프가 없는(Loop Free)  3계층 : TTL(Time To Live)

- 패킷이 네트워크에 살아 있을 수 있는 시간(홉)을 제한
- 라우팅 루프 : 순간적으로 마주보는 두 대의 라우터의 넥스트 홉이 각각 상대방으로 구성되어 패킷이 두 라우터 사이에서 계속 오가는 경우 발생
- TTL : 수명 값, 하나의 홉을 지날 때마다 TTL 값 1 감소

## 2. 라우팅(라우터가 경로 정보를 얻는 방법)

### 1. 다이렉트 커넥티드(Direct Connected)

- 해당 네트워크에 대한 라우팅 테이블을 자동으로 제작
- 정보를 강제 삭제 금지
- 해당 네트워크 설정 삭제, 해당 네트워크 인터페이스 비활성화 → 자동으로 삭제

### 2. 스태틱 라우팅(Static Routing)

- 관리자가 목적지 네트워크와 넥스트 홉을 라우터에 직접 지정해 경로 정보를 입력하는 것
- 라우팅 정보를 매우 직관적으로 설정, 관리 가능
- 연결된 인터페이스 정보가 삭제되거나 비활성화 → 연관된 스태틱 라우팅 정보 자동 삭제

### 3. 다이나밍 라우팅(Dynamic Routing)

- 라우터끼리 자신이 알고 있는 경로 정보나 링크 상태 정보를 교환해 전체 네트워크 정보 학습
- 관리자의 개입 없이 라우터끼리 정보교환 → 장애 인지 및 트래픽 우회
- 토폴로지 테이블 : 라우터가 수집한 경로 정보, 원시 데이터(Raw Data)
- 라우팅 테이블 : 경로 정보 중 최적의 경로를 저장하는 테이블

## 3. 스위칭(라우터가 경로를 지정하는 방법)

- 패킷이 들어와 라우팅 테이블을 참조하고 최적의 경로를 찾아 라우터 외부로 포워딩하는 작업
- 라우터가 패킷 경로를 지정해 보내는 작업
- 롱기스트 프리픽스 매치(Longest Prefix Match) : 갖고 있는 경로 정보 중 가장 가까운 경로 선택
    - 자신이 갖고 있는 라우팅 테이블에서 가장 좋은 항목을 찾는 알고리즘
    - LPM 테이블 : 라우터나 스위치에서 관리할 수 있는 라우팅 테이블
- 단순히 목적지 IP만 캐시하는 경우
- 출발지와 목적지 IP 모두 캐시하는 경우
- 포트 번호 정보까지 포함해 플로를 모두 저장하는 경우
- 넥스트 홉 L2 정보까지 캐시해 스위칭 시간을 줄이는 기법

## 4. 라우팅, 스위칭 우선순위

- 정보를 얻은 소스에 따라 가중치 달라짐
    - 내가 갖고 있는 네트워크(다이렉트 커넥티드)
    - 내가 경로를 직접 지정한 네트워크(스태틱 라우팅)
    - 경로를 전달받은 네트워크(다이나믹 라우팅)
- AD(Administrative Distance) : 필요에 따라 관리자가 우선순위를 조정해 라우팅 경로 조정
    - 0 : Connected Interface(다이렉트 커넥티드)
    - 1 : Static Route(스태틱 라우팅)
    - 20 : External BGP
    - 110 : OSPF
    - 115 : IS-IS
    - 120 : RIP
    - 200 : Internal BGP
    - 255 : Unknown
- 코스트(Cost), ECMP(Equal-Cost Multi-Path) → 트래픽 분산
- 롱기스트 프리픽스 매치 기법 → 우선순위 결정

# 3. 라우팅 설정 방법

## 1. 다이렉트 커넥티드

- 목적지가 외부 네트워크인데 다이렉트 커넥티드 라우팅 테이블 정보만 있으면 외부 네트워크와 통신 불가능
- 커넥티드 라우팅 정보만 있는 경우뿐만 아니라 외부 네트워크에 대한 라우팅 정보가 있더라도 다이렉트 커넥티드 정보를 잘못 입력하면 외부 네트워크와 통신 불가능

## 2. 스태틱 라우팅

- 네트워크 정보를 쉽게 추가하고 경로를 직접 제어
- 네트워크 관리자뿐만 아니라 서버 담당자도 경로 관리에 사용하는 경우가 많으므로 서버 관리자도 스태틱 라우팅을 잘 알아야함
- `ip route NETWORK NETMAST NEXTHOP` : 시스코 네트워크 장비
- `route add -net NETWORK /Prefix gw NEXTHOP` : 리눅스 서버 운영 체제
    - 목적지(네트워크/호스트 - 서브넷/서브넷 마스크)로 가려면 패킷을 넥스트 홉으로 보내야 함
- 대용량의 인터넷 라우팅 전용 라우터 필요
- 스태틱 라우팅을 확장한 디폴트 라우팅 사용
    - 디폴트 라우팅 : 목적지 주소의 서브넷 마스크가 모두 0인 스태틱 라우팅
- 모든 네트워크 : 모든 네트워크 정보를 체크하지 않는다는 의미
- 인터넷으로 향하는 경로나 자신에게 경로 정보가 없는 경우, 마지막 대체 경로로 디폴트 라우팅 사용

## 3. 다이나믹 라우팅

- SPoF(Single Point of Failure) : 단일 장애점
- 관리자의 직접적인 개입 없이 라우터끼리 정보를 교환해 경로 정보를 최신으로 유지 가능
- 유니캐스트 라우팅 프로토콜 : RIP, IGRP, OSPF, IS-IS, EIGRP, BGP
- 멀티캐스트 라우팅 프로토콜 : DVMRP, MOSPF, PIM

### 1. 역할에 따른 분류

- 유니캐스트 라우팅 프로토콜 : 일반적인 라우팅 프로토콜
- AS(Autonomous System) : 자율 시스템 존재
- IGP : AS 내에서 사용하는 라우팅 프로토콜
- EGP : AS 간 통신에 사용하는 라우팅 프로토콜
- AS 내부의 연결은 효율성이 중요하지만 다른 AS와의 연결에서는 효율성보다 조직 간 정책이 더 중요

### 2. 동작 원리에 따른 분류

- 디스턴스 벡터(Distance Vector) : 인접한 라우터에서 경로 정보를 습득하는 라우팅 프로토콜
    - 인접한 라우터에서 경로 정보 습득
    - 인접 라우터가 이미 계산한 결과물인 라우팅 테이블을 전달받고 계산 → 라우팅 정보 처리에 많은 리소스 필요 없음
- 링크 스테이트(Link-State) : 라우터에 연결된 링크 상태를 서로 교환하고 각 네트워크 맵을 그리는 라우팅 프로토콜
    - 최적의 경로를 연산한 결과물인 라우팅 테이블과 달리 직접적인 상태 정보 받아볼 수 있음
    - SPF(Shortest Path First) 알고리즘 → 최단 경로 트리
    - 전체 네트워크 맵, 경로 변화 파악 유리
    - 네트워크 에어리어 단위 분리
    - 분리된 에어리어 내에서만 링크 상태 정보 교환
