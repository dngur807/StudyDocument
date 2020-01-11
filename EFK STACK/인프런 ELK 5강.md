### 실데이터 분석 ELK 활용

데이터는 catalog.data.gov/에서 다운받자

population by country 에서 csv 다운받자!!

![image-20200109160604016](C:\Users\whjung\AppData\Roaming\Typora\typora-user-images\image-20200109160604016.png)



kibana 잘 돌아가는지 확인

ps -ef | grep kibana

ps-ef | grep elasticsearch

vi ~/git/BigData/ch06/logstash.conf 으로 확인

 start_position => "beginning" 튜토리얼이라 처음 부터 진행 (기본은 END => 스트리밍 데이터를 항상 받아야하니..)

sincedb_path => "/dev/null" 설정 안하면

처음에는 잘들어가지만 두번째는 한번 들어간 데이터를 넣지 않음...



필터는 csv 는 데이터는 ,로 되어 있습니다.

1980 2010 까지 설정하기 위해서  float으로 잡았고



output은 Elasticsearch로 보내기 위해 설정한것을 볼 수 있다.

인덱스는 population으로 설정

돌리자!

cd /usr/share/logstash/bin

 ./logstash -f ~/git/BigData/ch06/logstash.conf 



csv파일 input을 받아 필터에서 텍스트를 넘버로 바꿔서 아웃풋을 ES로 변경해서 전달하는 과정이다.

이제 키바나로 가보자 ES 에 데이터가 들어갔으니...



인덱스  population  입력

데이터 제대로 들어간거 확인

discover에서 확인

1980 ~ 2010 까지 잘있네

나라 인구수만 보고싶어 토글 ㄱㄱ



![image-20200109165543411](C:\Users\whjung\AppData\Roaming\Typora\typora-user-images\image-20200109165543411.png)



