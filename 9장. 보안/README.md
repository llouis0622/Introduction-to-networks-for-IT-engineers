# 9장. 보안

# 1. 보안의 개념과 정의

## 1. 정보 보안의 정의

- 다양한 위협으로부터 보안을 보호하는 것
- 기밀성(Confidentiality) : 인가되지 않은 사용자가 정보를 보지 못하게 하는 모든 작업
- 무결성(Integrity) : 정확하고 완전한 정보 유지에 필요한 모든 작업
    - MD5, SHA와 같은 해시(Hash) 함수를 이용해 변경 여부 파악
- 가용성(Availability) : 정보가 필요할 때 접근을 허락하는 일련의 작업
- 진정성(Authenticity), 책임성(Accountability), 부인 방지(Non-Repudiation), 신뢰성(Reliability)

## 2. 네트워크의 정보 보안

- IT 종사자 입장 : 정보를 수집, 가공, 저장, 검색, 송수신하는 도중에 정보의 훼손, 변조, 유출을 막기 위한 관리적, 기술적 방법
- 네트워크 입장 : 수집된 정보를 침해하는 행동을 기술적으로 방어하거나 정보의 송수신 과정에 생기는 사고를 막기 위한 작업

## 3. 네트워크 보안의 주요 개념

- 트러스트(Trust) 네트워크 : 외부로부터 보호받아야 할 네트워크
- 언트러스트(Untrust) 네트워크 : 신뢰할 수 없는 외부 네트워크
- 인터넷 시큐어 게이트웨이(Internet Secure Gateway) : 트러스트 네트워크에서 언트러스트 네트워크로의 통신 통제
    - 정보와 요청 패킷을 적절히 인식하고 필터링
    - SWG(Secure Web Gateway), 웹 필터(Web Filter), 애플리케이션 컨트롤(Application Control), 샌드박스(Sandbox)
- 데이터 센터 시큐어 게이트웨이(Data Center Secure Gateway) : 언트러스트 네트워크에서 트러스트로의 통신 통제
    - 상대적으로 고성능 필요
    - 인터넷 관련 정보보다 공격 관련 정보가 더 중요
    - 방화벽, IPS, DCSG(DataCenter Secure Gateway), WAF(Web Application Firewall), Anti-DDoS(Distribute Denial of Service)

### 1. 네트워크 보안 정책 수립에 따른 분류

- 화이트리스트(White List) : 방어에 문제가 없다고 명확히 판단되는 통신만 허용
    - 일반적으로 IP와 통신 정보에 대해 명확히 하는 경우에 사용
- 블랙리스트(Black List) : 공격이라고 명확히 판단되거나 문제가 있었던 IP 리스트나 패킷 리스트를 기반으로 데이터베이스를 만들고 그 정보를 이용해 방어하는 형태
    - 공격 패턴(Signature)

### 2. 정탐, 오탐, 미탐(탐지 에러 타입)

|  | 공격 상황 | 정상 상황 |
| --- | --- | --- |
| 공격 인지 | True Positive(정상 탐지) | False Positive(오 탐지) |
| 정상 인지 | True Negative(미 탐지) | False Negative(정상 탐지) |

# 2. 보안 솔루션의 종류

- DDos → 방화벽 → IPS → WAF 형태

## 1. DDoS 방어 장비

- Denial of Service
- 다양한 방법으로 공격 목표에 서비스 부하를 가해 정상적인 서비스를 방해하는 공격 기법
- 데이터 센터 네트워크 내부와 외부의 경계에서 공격 방어
- 볼류메트릭 공격(Volumetric Attack) : 회선 사용량이나 그 이상의 트래픽을 과도하게 발생시켜 회선 사용을 방해하는 공격

## 2. 방화벽

- 3, 4계층 정보를 기반으로 정책
- 해당 정책과 매치되는 패킷이 방화벽을 통과하면 그 패킷을 허용하거나 거부

## 3. IDS, IPS

- IDS(Intrusion Detection System) : 침입 탐지 시스템
- IPS(Intrusion Prevention System) : 침입 방지 시스템
- 방화벽에서 방어할 수 없는 다양한 애플리케이션 공격을 방어하는 장비

## 4. WAF(Web Application Firewall)

- 웹 서버를 보호하는 전용 보안 장비
- HTTP, HTTPS처럼 웹 서버에서 동작하는 웹 프로토콜의 공격 방어
- 전용 네트워크 장비
- 웹 서버 플러그인
- ADC 플러그인
- 프록시 장비 플러그인
- IPS 회피 공격(Evasion Attack) 방어

## 5. 샌드박스

- APT(Advanced Persistent Threat) : 지능형 지속 공격
- ATA(Advanced Target Attack) : 지능형 표적 공격
- 악성 코드를 샌드박스 시스템 안에서 직접 실행

## 6. NAC(Network Access Control)

- 네트워크에 접속하는 장치 제어
- 인가된 사용자만 내부망에 접속할 수 있고 인가받기 전이나 승인에 실패한 사용자는 접속할 수 없도록 제어

## 7. IP 제어

- NAC 솔루션과 공통적인 기술 이용, 기능 비슷
- IP 할당 및 추적
- 정확히 의도된 IP 할당이 아니면 정상적으로 네트워크를 사용하지 못하게 하는 기능

## 8. 접근 통제

- 서버나 데이터베이스에 대한 직접적인 접근을 막고 작업 추적 및 감사를 할 수 있는 접근 통제 솔루션
- 에이전트 기반, 에이전트리스, 배스천 호스트 기반
- 감사, 보안 이유 대응 등을 위해 사용자가 작업한 모든 이력 저장

## 9. VPN

- 사용자 기반의 VPN 서비스 제공
- IPSEC : 주로 네트워크 연결용
- SSLVPN : 사용자가 내부 네트워크에 연결
