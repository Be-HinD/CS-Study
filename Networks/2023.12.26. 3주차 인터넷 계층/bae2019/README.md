# 네트워크 계층

- IP 프로토콜 구조
    
   &emsp; -IPv4프로토콜
    
   &emsp;&emsp; ㄴ네트워크 상에서 데이터 교환 위한 프로토콜
    
   &emsp;&emsp; ㄴ데이터 정확한 전달 보장 X (중복패킷 전달, 순서 잘못 전달 가능)
    
   &emsp;&emsp; ㄴ>데이터의 정확, 순차적 전달은 TCP에서 보장
    
   &emsp; -구조
    
   &emsp;&emsp; ![Untitled](Untitled.png)
    
   &emsp;&emsp; ㄴ옵션 추가시 4byte씩 추가
    
   &emsp;&emsp; ㄴHeader Length : 전체 길이 / 4로 표현
    
   &emsp;&emsp; ㄴTotal Length : payload까지의 전체 길이
    
   &emsp;&emsp; ㄴIP Flag : 패킷 구분, 3bit(M-남은 패킷 유무, D-조각화 여부)
    
   &emsp;&emsp; ㄴprotocol : 상위 프로토콜 타입
    
   &emsp;&emsp; ㄴChecksum : 오류 검사
    
   &emsp; -IPv4 주소 : 32bit, 8bit씩 끊어서 4개로 표시
    
   &emsp;&emsp; ㄴIP주소 : 192.168.60.14 = Network Id + Host Id
    
   &emsp;&emsp; 길이 32bit = Network Id 길이 + Host Id 길이
    
   &emsp;&emsp; ㄴNet mask(SubNet Mask) : Network Id 길이, 255.255.255.0
    
   &emsp;&emsp; *192 → 1100 0000(2)
    
   &emsp;&emsp; *255 → 1111 1111(2)
    
   &emsp;&emsp; AND ⇒ 1100 0000(2) —> 192.168.60.0 = Network Id
    
   &emsp;&emsp; ㄴ255.255.255.0 → 24bit이므로 192.168.60.14/24로 Net mask 표기
    
   &emsp; -IPv6 주소 : 128bit
    
- ICMP 프로토콜
    
   &emsp; -ICMP 프로토콜 : 인터넷 제어 메시지 프로토콜
    
   &emsp;&emsp; ㄴ운영체제에서 오류 메시지 송신(type, code 이용)
    
   &emsp; -구조
    
   &emsp;&emsp; ![Untitled](Untitled1.png)
    
   &emsp;&emsp; ㄴType : 0(응답), 8(요청), 3(목적지접근X), 11(시간초과), 5(redirect-보안)
    
   &emsp;&emsp; ㄴCode : 소분류
    
   &emsp;&emsp; ㄴChecksum : 오류 검사
    
- 라우팅 테이블
    
   &emsp; -라우팅 테이블 : 프로토콜로 전송 시 경로 저장
    
   &emsp;&emsp; ㄴ라우팅 테이블에 적힌 네트워크 대역에만 송신
    
   &emsp;&emsp; ![Untitled](Untitled2.png)
    
   &emsp;&emsp; ㄴ0.0.0.0 → 기본 게이트웨이
    
   &emsp; - 통신 과정
        
   &emsp;     -기본
        
   &emsp;&emsp;     ![Untitled](Untitled3.png)
        
   &emsp;     - A에서 요청
            
   &emsp;&emsp;         -A의 라우팅 테이블
            
   &emsp;&emsp;&emsp;         192.168.0.0/24 → 192.168.10.1
            
   &emsp;&emsp;&emsp;         192.168.20.0/24 → 192.168.10.1
            
   &emsp;&emsp;         -ICMP : 요청08, 응답00
            
   &emsp;&emsp;&emsp;         ![Untitled](Untitled4.png)
            
   &emsp;&emsp;         -IPv4
            
   &emsp;&emsp;&emsp;         ![Untitled](Untitled5.png)
            
   &emsp;&emsp;         -이더넷 : 목적지 MAC주소는 바로 다음 공유기
            
   &emsp;&emsp;&emsp;         ![Untitled](Untitled6.png)
            
   &emsp;     - 다음 공유기 요청
            
   &emsp;&emsp;         -공유기의 라우팅 테이블
            
   &emsp;&emsp;&emsp;         192.168.10.0/24 → 192.168.10.1
            
   &emsp;&emsp;&emsp;         192.168.20.0/24 → 192.168.30.2
            
   &emsp;&emsp;         -IPv4
            
   &emsp;&emsp;&emsp;         ![Untitled](Untitled7.png)
            
   &emsp;&emsp;         -이더넷
            
   &emsp;&emsp;&emsp;         ![Untitled](Untitled8.png)
            
   &emsp;&emsp;         ⇒IP주소 & 라우팅 테이블 → 이더넷 주소 갱신
            
   &emsp;&emsp;&emsp;         ![Untitled](Untitled9.png)
            
        
   &emsp;&emsp;     →이더넷 프로토콜은 네트워크 별로 갱신
        
- 조각화 이론
    
   &emsp; -조각화 : 큰 IP 패킷이 적은 MTU(Maximum Transmission Unit) 갖는 링크로 전송 시 여러 작은 패킷으로 쪼개어 전송
    
   &emsp;&emsp; ㄴ목적지까지 패킷 전달 과정의 각 라우터마다 프레임으로 변환
    
   &emsp;&emsp; ㄴ보통 최종 목적지까지 재조립X
    
   &emsp;&emsp; ㄴIPv4 : 발신지, 중간 라우터 IP 조각화
    
   &emsp;&emsp; ㄴIPv6 : 발신지만 IP 조각화, 최종 수신지만 재조립
    
   &emsp; -과정
    
   &emsp;&emsp; ![Untitled](Untitled10.png)
    
   &emsp;&emsp; ㄴMTU 3,300 byte - header 20byte = payload 3,280byte
    
   &emsp;&emsp; ㄴMF : more fragment, 이후 패킷 더 있으면 1, 없으면 0
    
   &emsp;&emsp; ㄴOffset : 데이터 시작 위치 = 이전 데이터 크기 / 8 = 3,280/8 = 410
    
   &emsp; -데이터 조각화 후 헤더 붙이기
    
   &emsp;&emsp; ![Untitled](Untitled11.png)
    
   &emsp;&emsp; ㄴIPv4
    
   &emsp;&emsp; ![Untitled](Untitled12.png)
    

질문

🚨 structure, function, checksum

-IP 프로토콜과 ICMP 프로토콜의 구조에서 공통으로 존재하는 checksum의 기능을 설명하세요

→전송 전 데이터와 전송 후 데이터 간에 손상이 있는지 체크

+IPv6는 헤더에서 checksum 삭제 → TCP 단에도 있기 때문

🚨 경리 키워드 : 라우팅 테이블

-라우팅 테이블을 설명해주세요?

→각 라우터에 있는 데이터로서, 해당 정보를 이용해서 라우터는 네트워크 상의 목적지 주소를 목적지 도달 위한 네트워크 노선으로 변환시키는 기능 수행

→목적지와 거기로 가려면 어디로 전송해야 할 지 경로 저장

🚨 주용 키워드: subnet, reason, mask

-subnet mask를 사용하는 이유를 설명하세요?

→Network Id와 Host Id를 구분하기 위해?

🚨 승현 키워드: IPv4 IPv6

-IPv4와 IPv6의 차이를 설명하세요?

→IPv4는 32bit IP 주소, IPv6은 128bit IP 주소

→IPv4는 .으로 구분, IPv6은 :으로 구분

→IPv4는 checksum 있음, IPv6은 없음

→IPv4는 브로드캐스트 지원, IPv6은 지원X

→IPv4는 가변 길이 서브넷 마스크(VLSM) 지원, IPv6은 지원X

→MAC 주소 매핑 시 이용하는 프로토콜 다름

참고 : [https://www.guru99.com/ko/difference-ipv4-vs-ipv6.html](https://www.guru99.com/ko/difference-ipv4-vs-ipv6.html)

🚨 지훈 키워드: 조각화 id flag offset

-조각화 시 id flag와 offset의 기능을 설명하세요?

→ip flag : 조각화 여부 확인 bit와 이후 해당 패킷이 마지막 패킷인지 구분하기 위한 bit 등

→offset : 해당 패킷의 첫 데이터의 조각화 이전 데이터 시작점과의 거리

ㄴ전체 데이터그램에서 해당 패킷 데이터의 시작 위치
