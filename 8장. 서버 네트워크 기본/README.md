# 8장. 서버 네트워크 기본

# 1. 서버의 네트워크 설정 및 확인

## 1. 리눅스 서버 네트워크

- 설정 파일이 텍스트 형태 → 직접 수정해 시스템 구성 변경

### 1. CentOS의 네트워크 설정

- CentOS 네트워크 설정 파일 경로 : `/etc/sysconfig/network-scripts`
- 인터페이스 설정 항목
    - `ONBOOT` : 부팅 시 인터페이스를 활성화시킬 것인지 결정(yes/no)
    - `BOOTROTO` : 부팅 시 사용할 프로토콜(none, dhcp, static)
    - `IPADDR` : IP 주소
    - `NETMASK` : 서브넷 마스크
    - `PREFIX` : 서브넷 마스크(비트 값으로 표기)
    - `GATEWAY` : 게이트웨이 주소
    - `DNS1` : 주 DNS 정보 입력
    - `DNS2` : 보조 DNS
- 네트워크 서비스 재시작 : `systemctl restart network.service`
- 특정 인터페이스 다운/업(재시작) : `ifdown ifcfg-eth0` , `ifup ifcfg-eth0`

### 2. 우분투의 네트워크 설정

- 우분투 네트워크 설정 파일 : `/etc/network/interfaces`
- 우분투 네트워크 서비스 시작/정지/재시작
    - `/etc/init.d/networking start`
    - `/etc/init.d/networking stop`
    - `/etc/init.d/networking reload`
    - `/etc/init.d/networking restart`
    - `/etc/init.d/networking force-reload`

## 2. 윈도우 서버 네트워크

- 네트워크 어댑터 설정을 위한 netsh 명령 : `netsh interface ipv4 set address name="인터페이스명" static IP 주소 서브넷 게이트웨이`
- 윈도우 어댑터 비활성화 : `netsh interface set interface name="인터페이스명" admin=disabled`
- 윈도우 어댑터 활성화 : `netsh interface set interface name="인터페이스명" admin=enabled`

# 2. 서버의 라우팅 테이블

- 외부 공인망 네트워크와 통신해야 하는 프론트엔드 네트워크 쪽 어댑터에만 디폴트 게이트웨이를 설정, 백엔드를 연결하는 어댑터에는 별도로 적절한 라우팅 정보 반드시 설정

## 1. 서버의 라우팅 테이블

- 목적지 네트워크, 서브넷 : 서버가 통신하려는 목적지 IP 주소에 맞는 라우팅을 선택하는 기준
- 게이트웨이 : 선택된 목적지로 가지 위해 서버에서 선택하는 넥스트 홉
- 인터페이스 : 서버의 네트워크 카드, 라우팅에서 어떤 물리적인 경로로 패킷을 보낼지 설정
- 우선순위 : 동일한 라우팅 테이블이 두 개 이상 존재할 때 어떤 라우팅 테이블을 선택할지 결정

## 2. 리눅스 서버의 라우팅 확인 및 관리

- 라우팅 테이블 추가 : `route add { -host | -net } Target[/prefix] [gw Gw] [metric M] [[dev] If]`
- 라우팅 테이블 삭제 : `route del { -host | -net } Target[/prefix] [gw Gw] [metric M] [[dev] If]`

### 1. CentOS의 영구적 라우팅 설정

- CentOS 리눅스 라우팅 설정 파일 : `/etc/sysconfig/network-scripts/route-장치명`

### 2. 우분투의 영구적 라우팅 설정

- `up route add [-net|-host] <host/net>/<mask> gw <host/IP> dev <Interface>`

## 3. 윈도우 서버의 라우팅 확인 및 관리

- 윈도우 라우팅 테이블 : `route print`
    - `*` : 전체 문자열 대체
    - `?` : 특정 문자 하나 대체
- 라우팅 테이블 추가 : `ROUTE [-p] ADD [dest] [MASK netmask] [gateway] [METRIC metric] [IF interface]`
- 라우팅 테이블 삭제 : `ROUTE DELETE [dest] [MASK netmask] [gateway] [METRIC metric] [IF interface]`
- 라우팅 테이블 변경 : `ROUTE CHANGE [dest] [MASK netmask] [gateway] [METRIC metric] [IF interface]`

# 3. 네트워크 확인을 위한 명령어

## 1. ping(Packet InterNet Groper)

- 네트워크 상태 확인
- IP 네트워크를 통해 특정 목적지까지 네트워크가 잘 동작하고 있는지 확인
- `ping [옵션] 목적지_IP 주소`

## 2. tcping(윈도우)

- 서비스 포트가 정상적으로 열려 있는지 확인 가능
- `tcping [옵션] 목적지_IP 주소`

## 3. traceroute(리눅스) / tracert(윈도우)

- 출발지부터 통신하거나 목적지까지의 네트워크 경로 확인
- 출발지 PC에서 목적지까지의 라우팅 경로 정보 확인
- 문제 발생 구간 확인 가능
- `traceroute/tracert [옵션] 목적지_IP 주소`

## 4. tcptraceroute

- 서비스 포트를 추가로 확인
- `tcptraceroute [옵션] 목적지_IP 주소 [서비스 포트]` (리눅스)
- `tcproute [옵션] 목적지_IP 주소` (윈도우)

## 5. netstat(network statistics)

- 서버의 다양한 네트워크 상태 확인
- LISTENING(열려 있음), ESTABLISHED(세션 정상 작동), TIME_WAIT, FIN_WAIT, CLOSE_WAIT(정상 종료)
- `netstat [옵션]`

## 6. ss(socket statistics)

- 소켓 정보 확인
- `ss [옵션] [필터]`

## 7. nslookup(name server lookup)

- DNS에 다양한 도메인 관련 내용을 질의해 결괏값을 전송
- 기본 네임 서버를 사용한 대화형 모드 : `nslookup [옵션]`
- 기본 네임 서버를 server로 지정한 대화명 모드 : `nslookup [옵션] - server`
- 기본 네임 서버를 사용한 host 질의 : `nslookup [옵션] host`
- 기본 네임 서버를 server로 지정한 host 질의 : `nslookup [옵션] host server`

## 8. telnet(tele network)

- 원격지 호스트에 터미널 연결을 위해 사용
- `telnet 목적지 IP 서비스 포트`

## 9. ipconfig

- 네트워크 설정을 확인하는 윈도우 명령어
- 네트워크 주소 해제 : `ipconfig /release`
- 네트워크 주소 갱신 : `ipconfig /renew`
- 로컬 캐시 정보 삭제 : `ipconfig /flushdns`
- 도메인 캐시 정보 확인 : `ipconfig /displaydns`

## 10. tcpdump

- 네트워크 인터페이스로 오가는 패킷 캡처
- 장애 처리나 패킷 분석 필요 시 사용
