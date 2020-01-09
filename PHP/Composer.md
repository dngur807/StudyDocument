# Composer

> 출처   https://opentutorials.org/course/3018/5221 

컴포저란 의존성 관리 도구라 할 수 있음 여러가지 형태에 라이브러리 를 사용할 때 라이브러리 간에 의존성을 관리하는 도구

라이브러리를 다운받고 압축풀는 것을 컴포저가 대신해서 라이브러리 패키지를 가져올 수 있고 다운받을 수 있음



#### 설치

Composer 깔고 관리해야한다.

> 컴포저 홈페이지  https://getcomposer.org/  사용하는 기본적인 기능이있다.

* 유닉스 계열

로컬 : 프로젝트 안에 포함되서 다른곳에 배포했을 때 컴포저 설치할 필요 없음

> php composer-setup.php --install-dir=bin --filename=composer
>
> curl -sS https://getcomposer.org/installer | php

| 앞에서 수행하는 명령의 결과로 뒤에 (php) 입력에 전송

curl 로 해당 사이트에 파일을 다운받은 뒤 그 내용을 php 엔진에 입력으로 전달해서 php 아카이브 파일 생성 .phar 



글로벌은 운영체제 전체서 사용할수 것

> ```bash
> mv composer.phar /usr/local/bin/composer
> ```

php composer.phar 실행해보자

그럼 composer.phar 을 이용해서 컴포저 실행 가능

글로벌하게 설치해보자!



### Packagist

프로젝트에 패키지를 삽입하려면 필요한 패키지, 라이브러리를 찾아야지 

>  패키지 추가 방법 https://packagist.org/ 

```
composer.json에 원하는 패키지 추가
{
  "require" : {
  	   //vendor / 이름 		// 버전
      "dflydev/markdown" : "1.0.3"
  }
}
해당 폴더로 이동 후 composer install하면 vendor 만들어진다

```

패키지를 사용해보자!

> ```
> require_once '../vendor/autoload.php';
> use dflydev\markdown\MarkdownExtraParser;
> $markdown= new MarkdownParser();
> echo $markdown->transformMarkdown("#Hello World");
> 이렇게 명시하면 vendor에 추가한 패키지, 의존성 있는 라이브러리 들이 전부 자동으로 로딩된다. 
> use는 네임스페이스 관련된 기능이죠
> ```

composer.lock은 이전 설치된 파일 정보가 담겨있어서, 그래서 다시 composer install하면 이전 버전을 유지해서 추가된다.  그래서 옛날에 설치한 버전이 설치되는데 새로운 버전으로 업데이트 하고 싶으면 Update 하면 최신으로 업그레이드 된다. 

버전 관리 용이하기 위해