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

curl -XGET localhost:9200/classes/class/1/?pretty



doc에 파일에 저장할 수도 있고,

항상 커맨드라인에 치면 시간도 오래걸리고, 백업이나 복구 시킬때는 파일을 사용해야한다.

curl -XPOST localhost:9200/classes/class/1/?pretty -d @파일이름



