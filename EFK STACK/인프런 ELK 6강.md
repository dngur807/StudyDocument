### 클라우드 로그 분석 시스템 아키텍처



그럼 어떻게 로그 스태시는 클라우드 상에 로그를 입력 받을 까요 

파일 비트입니다 . 파일 비트는 각각 서버에  설치해서 변화된 로그를 로그 스태시로 전달

![image-20200109173555029](C:\Users\whjung\AppData\Roaming\Typora\typora-user-images\image-20200109173555029.png)



오래된 데이터를 사용 하지 않아 공간 문제가 발생할 수 있는데요,. 이건 큐레이터를 사용하면 됩니다. 손쉽게 데이터 기간 및 최대 사용을 설정해서 디스크 부족 문제를 발생하지 않게한다.

![image-20200109173718128](C:\Users\whjung\AppData\Roaming\Typora\typora-user-images\image-20200109173718128.png)

실제 한달 이상된 로그를 봐야하는 경우도 있겠지 이것을 대비해 매일 발생하는 로그는 SDB에 저장해서 필요시 한달이상 로그는 복구하면 보다 완벽한 로그이다..



#### FileBeat로 ELK 스택에 전달

#### 설치

 https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-installation.html 

```
curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.5.1-x86_64.rpm
sudo rpm -vi filebeat-7.5.1-x86_64.rpm
```



설정 파일

/etc/filebeat/filebeat.yml

ch07 에 있는거 복붙...

이제 보내는 파일 비트 설정은 끝났고



로그 스태쉬에서도 이 보낸 정보를 받아야하는데,

/etc/logstash/conf.d/logstash.conf 여기서 설정한다.

input에는 파일 비트 정보를 받는다는 것을 알 수 있고

받은 정보를 elasticsearch에 넣어줘야하지...

그건 output



모든 설정이 끝났으니 서비스 재시작 하자!

logstash , filebeat , elasticsearch , kibana

initctl restart logstash

```
systemctl restart kibana
systemctl restart elasticsearch
systemctl restart filebeat
```

./logstash -f /etc/logstash/conf.d/logstash.conf

 sudo filebeat -e -v