# ASP.NET과 Redis 랭킹 서버 만들기 -1

> 출처  http://lab.gamecodi.com/board/zboard.php?id=GAMECODILAB_Lecture_series&page=1&sn1=&divpage=1&sn=off&ss=on&sc=on&select_arrange=headnum&desc=asc&no=130 

강좌 순서 

1. Redis
   1. 윈도우 용 Redis 설치
   2. 클라이언트 테스트
2. 어플리케이션 구축
   1. 랭킹 구현을 위한 Redis 명령어 알아보기
   2. ASP.NET 프로젝트 구축
   3. 웹 서비스를 위한 IIS 설정
   4. API 테스트
   5. 유니티 클라이언트 테슽
3. 결론
   1. 보완 할 점
   2. 참고 자료

## 1. Redis

### 윈도우용 Redis 설치

Redis는 윈도우 운영체제를 정식으로 지원하지 않습니다. 하지만 윈도우용 레디스를 다운로드 할 수 있는 링크를 제공해 주고 있습니다. 

```
오픈 소스 윈도우용 레디스 https://github.com/MSOpenTech
```

```
설치 파일 https://github.com/MSOpenTech/redis/releases
최신 버전 문제 있어서 3.0.504 설치하자
```

설치 후 확인 

`윈도우 + R services.msc 입력`

<img src="D:\Study\StudyDocs\StudyDocument\images\1_1_REDIS.PNG" alt="1_1_REDIS"  />

실행 중 아니면 시작 누르자

### 클라이언트 테스트

레디스 정상적으로 설치되고 실행중인지 확인하기 위해서 클라이언트를 접속해 보겠습니다.

레디스 설치된 폴더로 가서 redis-cli.exe 파일을 실행합니다.

![1_2_REDIS](D:\Study\StudyDocs\StudyDocument\images\1_2_REDIS.PNG)

keys * [엔터] 눌러보자

### 2. 어플리케이션 서버 구축

#### 랭킹 구현을 위한 Redis 명령어 알아보기

레디스는 여러 가지 데이터를 저장하고 불러올 수 있는 서버 프로그램이다.

온라인 게임 서버를 만들어 봤거나 연동해본 분이라면 특별할 것 없는 소켓형태의 서버 처럼 보일 수 있다. 하지만 데이터 저장 검색을 위한 여러 가지 기능들과 자료구조가 이미 구현되어 있어서 이런 기능들ㅇ르 사용한다면 똑같은 것을 중복해서 만들 필요가 없다.

```
레디스 공식 사이트 https://redis.io 명령어 많이 있음 
```

```
ZADD : 멤버와 점수를 입력합니다.
ZINCRBY : 점수를 더한다
ZREVRANGE : 일정 범위의 랭킹 리스트를 가져온다.
ZREVRANK : 멤버의 순위를 가져온다.
```

테스트 해보자

```
zadd rank-bestscore 100 Jane
zadd rank-bestscore 200 Kelly
zadd rank-bestscore 300 Bill
zrevrange rank-bestscore 0 2
```

테스트 했던 데이터들을 모두 삭제하는 flushall 명령어를 입력해보자

