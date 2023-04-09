# 10장. 서버의 방화벽 설정 / 동작

# 1. 리눅스 서버의 방화벽 확인 및 관리

- 호스트 방화벽 기능 → iptables
    - netfilter를 이용할 수 있도록 해주는 사용자 공간 응용 프로그램

## 1. iptables 이해하기

- 서버에서 허용하거나 차단할 IP나 서비스 포트에 대한 정책 수립
- 서버로 유입되는 구간(INPUT)
- 서버에서 나가는 구간(OUTPUT)
- 서버를 통과하는 구간(FORWARD)
- 방향성에 따라 방향성과 관련된 정책 그룹으로 분류 → 역할과 관련된 정책 그룹으로 재분류
- 체인(Chain) : 개별 정책의 방향성에 따라 구분한 그룹
- 테이블(Table) : 체인을 역할별로 구분한 그룹
    - 필터 테이블, NAT 테이블, 맹글 테이블, 로 테이블, 시큐리티 테이블
- Filter 테이블
    - iptables에서 패킷을 허용하거나 차단하는 역할을 선언하는 영역
- INPUT, OUTPUT, FORWARD 체인
    - 호스트 기준으로 호스트로 들어오거나 호스트에서 나가거나 호스트를 통과할 때 사용되는 정책들의 그룹
    - 패킷의 방향성에 따라 각 체인에 정의된 정책이 적용
- Match
    - 제어하려는 패킷의 상태 또는 정보 값의 정의
    - 정책에 대한 조건
- Target
    - Match와 일치하는 패킷을 허용할지, 차단할지에 대한 패킷 처리 방식

## 2. 리눅스 방화벽 활성화 / 비활성화

- firewalld 비활성화 및 서비스 중단 : `systemctl disable firewalld` , `systemctl stop firewalld`
- iptables 서비스 설치 : `yum install iptables-services`
- iptables 서비스 활성화 및 시작하기 : `systemctl start iptables.service`
- CentOS 7에서 service 명령어로 iptables 활성화 : `service iptables start`

## 3. 리눅스 방화벽 정책 확인

- iptables 설정값 확인 : `iptables -L`
- INPUT 체인 1번 정책 : `ACCEPT all-- anywhere anywhere state RELATED, ESTABLISHED`
    - 이미 세션이 맺어져 있거나(ESTABLISHED) 연계된 세션이 있을 떄, 어떤 출발지나 목적지인 패킷이더라도 허용하는 정책
- INPUT 체인 2번 정책 : `ACCEPT icmp-- anywhere anywhere`
    - ICMP에 대한 허용 정책
- INPUT 체인 4번 정책 : `ACCEPT tcp-- anywhere anywhere state NEW tcp dpt:ssh`
    - 신규 세션인 NEW state 중 목적지 서비스 포트가 SSH인 경우만 허용
    - 외부에서 서버로 SSH(22) 접속 허용하는 정책
- INPUT 체인 5번 정책 : `REJECT all-- anywhere anywhere reject-with icmp-host-prohibited`
    - 첫 번째부터 네 번째 정책에 매치되지 않은 패킷들을 차단하는 정책
- INPUT 체인 3번 정책 : `ACCEPT all-- anywhere anywhere`
    - 루프백 인터페이스에 대한 정책 모두 허용

## 4. 리눅스 방화벽 정책 관리

```
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
INPUT 체인에 정책 추가 + TCP 프로토콜 사용 + 목적지 서비스 포트 지정 + 타깃 지정
```

- iptables에 정책을 추가 → 반드시 적절한 위치 확인

## 💭. iptables 추가내용

- filter 테이블 : 패킷을 차단하거나 허용
- nat 테이블 : NAT 기능을 위한 테이블, Source NAT + Destination NAT
- mangle 테이블 : 주로 패킷 헤더의 TOS, TTL 값 변경
- raw 테이블 : 연결 추적 시스템(Connection Tracking System)에서 처리하면 안 되는 패킷 표시
- security 테이블 : 필수 접근 제어(Mandatory Access Control, MAC) 네트워크 규칙 사용
- 체인 : 특정 패킷에 대해 적용할 정책 정의
- 타깃 : iptables에 정의한 정책과 같을 때 취하는 행동
    - ACCEPT : 패킷을 정상적으로 처리
    - REJECT : 패킷을 폐기하면서 패킷이 차단되었다는 응답 메시지 전송
    - DROP : 패킷 그대로 폐기
    - LOG : 패킷을 syslog에 기록
