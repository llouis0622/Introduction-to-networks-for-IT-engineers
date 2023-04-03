# 7장. 통신을 도와주는 네트워크 주요 기술

# 1. NAT(Network Address Translation)/PAT(Port Address Translation)

- 네트워크 주소를 변환하는 기술
- IP 주소를 다른 IP 주소로 변환해 라우팅을 원활히 해주는 기술
- 1:1 변환이 기본, 1:N 변환도 가능
- AFT(Address Family Translation) : IPv4 주소와 IPv6 주소를 상호 변환하는 기술
- 사설 IP 주소에서 공인 IP 주소로 전환

## 1. NAT/PAT의 용도와 필요성

- IPv4 주소 고갈문제의 솔루션으로 NAT 사용
- 보안 강화 → 방향성 통제
- IP 주소 체계가 같은 두 개의 네트워크 간 통신 가능
    - 더블 나트(Double NAT) : IP 대역이 같은 네트워크와 통신할 가능성이 높은 대외계 네트워크를 연결하기 위해 출발지와 도착지를 한꺼번에 변환
- 불필요한 설정 변경 감소
    - 홀 펀칭(Hole Punching) : NAT 밑에 있는 단말 직접 연결

## 2. SNAT와 DNAT

- SNAT(Source NAT) : 출발지 주소를 변경하는 NAT
    - 사설에서 공인으로 통신 : 공유기처럼 PAT 사용
    - 보안 : 변경되는 IP가 반드시 공인일 필요 없음
    - 로드 밸런서 구성
- DNAT(Destination NAT) : 도착지 주소를 변경하는 NAT
    - 로드 밸런서
    - 사내가 아닌 대외망과의 네트워크 구성
- NAT가 수행되기 이전의 트래픽이 출발하는 시작 지점
- 역 NAT : NAT 장비를 처음 통과할 때 NAT 테이블이 생성 → 응답 패킷이 NAT 장비에 들어오면 별도의 NAT 설정이 없더라도 NAT 테이블을 사용해 반대로 패킷 변환

## 3. 동적 NAT와 정적 NAT

- 동적 NAT : 출발지나 목적지 어느 경우든 사전에 정해지지 않고 NAT를 수행할 때 IP를 동적으로 변경
    - 1:N, N:1, N:M 통신
    - NAT 테이블 NAT 수행 시 설정
    - NAT 테이블 타임아웃 동작
    - 실시간으로 확인하거나 별도 변경 로그 저장 필요
- 정적 NAT : 출발지와 목적지의 IP를 미리 매핑해 고정해놓은 NAT
    - 1:1 통신
    - NAT 테이블 사전 생성
    - NAT 테이블 타임아웃 없음
    - 별도 필요 없음 → NAT 내역이 설정

# 2. DNS(Domain Name System)

- 도메인 주소를 IP 주소로 변환
- 데이터 프로토콜 : 실제로 데이터를 실어나르는 것
- 컨트롤 프로토콜 : 데이터 프로토콜이 잘 동작하도록 도와주는 것
- MSA(Micro Service Architecture) 기반 서비스 설계 증가

## 1. DNS 소개

- 하나의 IP 주소를 이용해 여러 개의 웹 서비스 운영
- 서비스 중인 IP 주소 변경 → 도메인 주소 그대로 유지해 접속 방법 변경 없이 서비스 그대로 유지
- 지리적으로 여러 위치에서 서비스 가능
- 인터넷 연결을 위한 DNS와 내부 서비스 간의 이름 풀이와 통신 → 외부와 내부 DNS 분리 운영

## 2. DNS 구조와 명명 규칙

- 최상위 루트부터 Top-Level 도메인, Second-Level 도메인, Third-Level 도메인과 같이 하위 레벨로 원하는 주소를 단계적으로 찾아감
- 각 계층의 경계를 `.` 으로 표시, 뒤에서 앞으로 해석
    
    ```
    www.naver.com.
    www(Third-Level Domain)
    naver(Second-Level Domain)
    com(Top-Level Domain(TLD))
    맨 오른쪽 .(Root, 생략)
    ```
    
- 최대 128계층, 계층별 길이 최대 63바이트, 전체 도메인 네임 길이 최대 255바이트

### 1. 루트 도메인

- 도메인을 구성하는 최상위 영역
