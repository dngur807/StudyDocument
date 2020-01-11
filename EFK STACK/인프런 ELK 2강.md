#### ES 기본개념 정리

![image-20200109090839339](C:\Users\dngur\AppData\Roaming\Typora\typora-user-images\image-20200109090839339.png)

데이터 흐름이 있는데

교수님이 어떤 강의를 가르킨다고 하면

이것을 ES 에 저장하면 이렇게 저장하지





![image-20200109090924093](C:\Users\dngur\AppData\Roaming\Typora\typora-user-images\image-20200109090924093.png)

관계형 데이터 네이스랑 보면

전부다 저장하지

그래서 ES 가 바로 doc1,doc2 볼 ㅅ ㅜ있다.

관계형은 doc1에 json 살펴보고 알려주기 떄문에 속도가 느리다.

ES는 해쉬 테이블과 같죠 그래서 서치 관점에서 매우 빠르지

![image-20200109091220405](C:\Users\dngur\AppData\Roaming\Typora\typora-user-images\image-20200109091220405.png)

ES 자료구조를 보면

가장 큰개념이 Index고 Index에는 type 을 가지고 있고 type 안에서 여러가지 doc를 가지고 있다.

![image-20200109091352669](C:\Users\dngur\AppData\Roaming\Typora\typora-user-images\image-20200109091352669.png)

![image-20200109091410912](C:\Users\dngur\AppData\Roaming\Typora\typora-user-images\image-20200109091410912.png)

![image-20200109091433211](C:\Users\dngur\AppData\Roaming\Typora\typora-user-images\image-20200109091433211.png)





### Create , delete , update

![image-20200109091520610](C:\Users\dngur\AppData\Roaming\Typora\typora-user-images\image-20200109091520610.png)

먼저 인덱스를 만들건데, get이라는 레스트 엘프를 씁니다.

cur 커멘드로 보내는데 -X라는 프리픽스 붙는다.

데이터 조회 는 class 인덱스 있는지 조회

curl -XGET localhost:9200/classes

![image-20200109091757369](D:\Study\StudyDocs\StudyDocument\image-20200109091757369.png)

404 에러 볼 수 있다. 페이지 없다는 뜻이지...

curl -XGET localhost:9200/classes?pretty를 붙이면 결과를 깔끔하게 볼 수 있다.

![image-20200109091923490](C:\Users\dngur\AppData\Roaming\Typora\typora-user-images\image-20200109091923490.png)

인덱스가 없으니 생성을 해보자

curl -XPUT localhost:9200/classes

확인해보면

curl -XGET localhost:9200/classes?pretty

![image-20200109092025297](C:\Users\dngur\AppData\Roaming\Typora\typora-user-images\image-20200109092025297.png)



#### 인덱스 삭제

curl -XDELETE localhost:9200/classes?pretty

curl -XGET localhost:9200/classes?pretty



#### doc 만드는법

인덱스 있을 때 만들 수 있고 인덱스 없을 때도 인덱스 명과 타입을 명시해 주면 만들 수 있다.

인덱스 : classes 타입  : class 아이디 : 1

curl -XPOST localhost:9200/classes/class/1/?pretty -d '{"title" : "Algorithm" , "professor" : "John"}'



curl -XPOST localhost:9200/classes/class/1/?pretty -H 'Content-Type: application/json' -d '{"title" : "Algorithm" , "professor" : "John"}'





curl -XGET localhost:9200/classes/class/1/?pretty



doc에 파일에 저장할 수도 있고,

항상 커맨드라인에 치면 시간도 오래걸리고, 백업이나 복구 시킬때는 파일을 사용해야한다.

curl -XPOST localhost:9200/classes/class/1/?pretty -d @파일이름





#### 데이터 Update 

curl -XPOST http://localhost:9200/classes/class/1/_update?pretty -H 'Content-Type: application/json' -d '{"doc" : {"unit" : 1}}'

curl -XGET localhost:9200/classes/class/1/?pretty



업데이트 하는 방법이 하나 더 있다.

script 를 사용할 수 있다.

curl -XPOST http://localhost:9200/classes/class/1/_update?pretty -H 'Content-Type: application/json' -d '{"script" : "ctx._source.unit +=5 "}'





#### 벌크

여러개의 doc를 한번에 삽입하는 방법

![image-20200109105600183](C:\Users\whjung\AppData\Roaming\Typora\typora-user-images\image-20200109105600183.png)

명령어는 

curl -XPOST http://localhost:9200/_bulk?pretty --data-binary @파일이름

curl -XPOST http://localhost:9200/_bulk?pretty  -H 'Content-Type: application/json' --data-binary @classes.json

curl -XGET localhost:9200/classes/class/1/?pretty // 제대로 들어간거 확인해보자!!!



#### 매핑

관계형 데이터 베이스에 스키마랑 동일



매핑 없으면 위험 

데이트 넣는데, ES 매핑 없으면 날짜인지 모르니 문자로 저장할 수 도 있음

이런 것 때문에 시각화 할 때 제대로 안나올 수 도 있다.

![image-20200109110414630](C:\Users\whjung\AppData\Roaming\Typora\typora-user-images\image-20200109110414630.png)

매핑전에 classes 라는 인덱스를 만들꺼임

curl -XPUT http://localhost:9200/classes

curl -XGET localhost:9200/classes?pretty



![image-20200109110656052](C:\Users\whjung\AppData\Roaming\Typora\typora-user-images\image-20200109110656052.png)

결과를 보면 매핑자리에 없는 것을 볼 수 있다.

매핑 만들기

curl -XPUT http://localhost:9200/classes/class/_mapping -d @매핑 파일 이름

curl -XPUT 'http://localhost:9200/classes/class/_mappings?pretty' -H 'Content-Type: application/json' -d @classesRating_mapping.json



curl -H 'Content-Type:application/json' -XPUT 'http://localhost:9200/classes/class/_mapping?include_type_name=true&pretty' -d @classesRating_mapping.json



![image-20200109112708693](C:\Users\whjung\AppData\Roaming\Typora\typora-user-images\image-20200109112708693.png)

매핑 정보 채워진거 확인

curl -XGET localhost:9200/classes?pretty



매핑 완료했으니 인덱스에 실제 데이터를 넣어야겠죠

curl -H 'Content-Type: application/json'  -XPOST http://localhost:9200/_bulk?pretty  --data-binary @classes.json



curl -XGET localhost:9200/classes/class/1/?pretty





#### 데이터 조회 

curl -H 'Content-Type: application/json'  -XPOST http://localhost:9200/_bulk?pretty  --data-binary @simple_basketball.json

curl -XGET localhost:9200/basketball/record/_search?pretty

// 점수 30인것 만

curl -XGET 'localhost:9200/basketball/record/_search?q=points:30&pretty'



curl -H 'Content-Type:application/json' -XGET localhost:9200/basketball/record/_search?pretty -d '{ 

"query" : 

{

"term" : {"points" : 30

}  

}}'



#### AGGS

조합을 통해 어떤 값을 도출할 때 쓰입니다. 

예를 들때 평균을 구할 때 포맷은

![image-20200109113842883](C:\Users\whjung\AppData\Roaming\Typora\typora-user-images\image-20200109113842883.png)

docs에 데이터 추가하자

curl -XPOST http://localhost:9200/_bulk?pretty  -H 'Content-Type: application/json' --data-binary @simple_basketball.json

평균 aggs 을 보면

![image-20200109114032481](C:\Users\whjung\AppData\Roaming\Typora\typora-user-images\image-20200109114032481.png)



curl -H 'Content-Type:application/json' -XGET localhost:9200/_search?pretty --data-binary @avg_points_aggs.json

![image-20200109114307971](C:\Users\whjung\AppData\Roaming\Typora\typora-user-images\image-20200109114307971.png)

sum, min, max 등등 실습 가능 모든것을 한번에 도출하고 싶으면

stats를 사용하자

curl -H 'Content-Type:application/json' -XGET localhost:9200/_search?pretty --data-binary @stats_points_aggs.json



![image-20200109114458729](C:\Users\whjung\AppData\Roaming\Typora\typora-user-images\image-20200109114458729.png)



#### 어그리게이션 버켓

조합해서 나타내는 방법 뜻한다. 최소 최대 

버켓은 그룹 바이라고 보면 된다. 



basketball 추가하자

curl -XDELETE localhost:9200/basketball?pretty

curl -XPUT http://localhost:9200/basketball/ 인덱스 만들고

매핑 조회 하자 basketball_mapping.json

![image-20200109114804286](C:\Users\whjung\AppData\Roaming\Typora\typora-user-images\image-20200109114804286.png)

매핑 적용

curl -H 'Content-Type:application/json' -XPUT 'http://localhost:9200/basketball/record/_mapping?include_type_name=true&pretty' -d @basketball_mapping.json

curl -XGET localhost:9200/basketball/?pretty

이제 도큐멘트에 넣을거 보자

twoteam_basketball.json 사용할 거다.

curl -H 'Content-Type: application/json' -XPOST 'http://localhost:9200/_bulk?pretty'   --data-binary @twoteam_basketball.json

curl -XGET localhost:9200/basketball/record/_search?pretty





terms_aggs 보자 

size 0인 이유는 우리가 여러개 정보 도출 대신 원하는 정보만 보기위해서

aggs : 어그리게이션

​	players :

​		terms 어그리게이션이고요



![image-20200109120950582](C:\Users\whjung\AppData\Roaming\Typora\typora-user-images\image-20200109120950582.png)

curl -H 'Content-Type:application/json' -XGET localhost:9200/_search?pretty --data-binary @terms_aggs.json



![image-20200109121057320](C:\Users\whjung\AppData\Roaming\Typora\typora-user-images\image-20200109121057320.png)

이런 자료가 있으면 팀별로 성적 통계 분석 해보자

각 팀별로 그룹해서 통계보자

![image-20200109121212913](C:\Users\whjung\AppData\Roaming\Typora\typora-user-images\image-20200109121212913.png)

curl -H 'Content-Type:application/json' -XGET localhost:9200/_search?pretty --data-binary @stats_by_team.json