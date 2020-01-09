#### 인프런 ELK

1강 빅데이터 개발자

![image-20200109084956662](C:\Users\dngur\AppData\Roaming\Typora\typora-user-images\image-20200109084956662.png)

![image-20200109085021064](C:\Users\dngur\AppData\Roaming\Typora\typora-user-images\image-20200109085021064.png)

2. 엘라스틱 서치 설치 

1. jdm에서 돌아가서 jdk 설치해야한다.

![image-20200109085204214](C:\Users\dngur\AppData\Roaming\Typora\typora-user-images\image-20200109085204214.png)

2. 그런 다음 elasticsearch를 직접 인스톨 할 수 있다.

![image-20200109085239224](C:\Users\dngur\AppData\Roaming\Typora\typora-user-images\image-20200109085239224.png)

3. 인스톨 완료하면

![image-20200109085302683](C:\Users\dngur\AppData\Roaming\Typora\typora-user-images\image-20200109085302683.png)

4. 확인

![image-20200109085345685](C:\Users\dngur\AppData\Roaming\Typora\typora-user-images\image-20200109085345685.png)





ES 설치 ( https://www.elastic.co/guide/en/elasticsearch/reference/7.5/rpm.html#rpm-repo )

 https://www.elastic.co/kr/support/matrix  자신의 OS에서 돌아가는지 확인



```
/etc/yum.repos.d/elasticsearch.repo
[elasticsearch]
name=Elasticsearch repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=0
autorefresh=1
type=rpm-md

sudo yum install --enablerepo=elasticsearch elasticsearch 
systemctl start elasticsearch.service
```

