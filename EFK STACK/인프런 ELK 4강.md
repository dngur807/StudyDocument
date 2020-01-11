### 로그스태시 인스톨 셋업

![image-20200109143904658](C:\Users\whjung\AppData\Roaming\Typora\typora-user-images\image-20200109143904658.png)

로그 스태시는 인풋 담당

받아서 ES에 전달 kibana 시각화



로그 스태시는 데이터 포맷을 바꿀 수 있음

ex) csv 는 텍스트인데 수치적으로 빼고싶으면 수치로 변경할 수 있고

변환 후에는 ES에 넣을 수 있다.



java -version 으로 설치 확인 해야함.

wget https://artifacts.elastic.co/downloads/logstash/logstash-6.4.2.rpm 다운로드
yum install logstash-6.4.2.rpm

 컨피그 파일 필요합니다.

input , output , 필더 3개로 구성되어 있다.

![image-20200109144939134](C:\Users\whjung\AppData\Roaming\Typora\typora-user-images\image-20200109144939134.png)

ch06 파일에 있음

키보드 input 받고 

stdout 모니터로 출력 받겠다는 거죠

이제 실행을 해보자

 cd /usr/share/logstash/bin/

./logstash -f ~/git/BigData/ch06/logstash-simple.conf

실행후 입력 해보자

![image-20200109145548738](C:\Users\whjung\AppData\Roaming\Typora\typora-user-images\image-20200109145548738.png)