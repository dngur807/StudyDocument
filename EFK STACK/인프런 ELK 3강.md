#### 키바나 설치 및 설정

5.0.2 사용했네...

설정 파일 vi /etc/kibana/kibana.yml 수정

 http://localhost:5601/app/kibana#/home?_g=() 



#### 키바나 매니지 먼트

curl -XDELETE localhost:9200/basketball?pretty

curl -XPUT localhost:9200/basketball 인덱스 생성



매핑을 넣어줄껀데 넣어주는 이유는  데이트 스트링으로 저장하면 안되서

명시적으로 지정 해서 시각화 할 때 도움 받을 수있음.

curl -H 'Content-Type:application/json' -XPUT 'http://localhost:9200/basketball/record/_mapping?include_type_name=true&pretty' -d @basketball_mapping.json



docs 농구 자료 삽입할 것이다,.

curl -XPOST 'http://localhost:9200/_bulk?pretty'  -H 'Content-Type: application/json' --data-binary @bulk_basketball.json





자료 있으니 한번 이 자료로 kibana에서 살펴보자

kibana에서 management에서 basketball 로 변경 후 진행

![image-20200109123909210](C:\Users\whjung\AppData\Roaming\Typora\typora-user-images\image-20200109123909210.png)



submit_date 필드를 타임 필드로 사용한다고 하고 실행하자

![image-20200109123925132](C:\Users\whjung\AppData\Roaming\Typora\typora-user-images\image-20200109123925132.png)

![image-20200109124054106](C:\Users\whjung\AppData\Roaming\Typora\typora-user-images\image-20200109124054106.png)



이제 베스켓볼 인덱스가 키바나에서 표시되는 것을 볼 수 있다.

매핑 그대로 잘 되있네요



#### 디스커버

살펴보면 Noresults 로 나온다 왜?? 데이터가 지난 15분간 없어서...

타임 필터를 2016년 데이터 볼수있게 바꾸자



vertical bar

![image-20200109140424043](C:\Users\whjung\AppData\Roaming\Typora\typora-user-images\image-20200109140424043.png)



pie char



![image-20200109140744630](C:\Users\whjung\AppData\Roaming\Typora\typora-user-images\image-20200109140744630.png)

#### 타일맵 지도에 표시

 curl -XPUT http://localhost:9200/classes 인덱스 생성

매핑은 지도에 표시하는데 필요하다.

curl -H 'Content-Type:application/json' -XPUT 'http://localhost:9200/classes/class/_mapping?include_type_name=true&pretty' -d @classesRating_mapping.json

curl -XGET localhost:9200/classes/?pretty 확인

school_location type을 geo_point 라고 준것 확인



curl -H 'Content-Type: application/json'  -XPOST http://localhost:9200/_bulk?pretty  --data-binary @classes.json



kibana로 넘어가서 index pattern 변경

visualize -> map을 선택

bucket에서 Go coordinates 사용

school_localhost 을 필드로 해서 표시하자

![image-20200109141935542](C:\Users\whjung\AppData\Roaming\Typora\typora-user-images\image-20200109141935542.png)

#### 대시보드

1. visualize 에서 vertical cart 하나 만들고  교수 평균 강의 평가 저장하자

2. 대시보드에서 재사용할 수 있다.
3. 대시보드로 가서 ADD 에서 만든 차트를 불러올 수 있다.

