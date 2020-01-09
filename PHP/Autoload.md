# PHP Auto Load



> 출처  https://freehoon.tistory.com/75 

가볍게 클래스를 하나 만들어 보자!

```
class Hello
{
	public function func()
	{
		echo "하이";
	}
}
```

이렇게 만들어서 호출하면 아무것도 안나오죠 해당 매소드를 사용하지 않아서 그럼 

index.php 에 이를 호출해보자

```
require_once 'Hello'
$sayHello = new Hello();
$sayHello->func();
```

이제 제대로 출력되네요  근데 이것외에도 여러 함수가 있고 호출할때마다 포함시켜야하는데 번거롭지!!

또 일이 늘어놔서 모든 클래스를 require했는데 중간에 빼먹으면 오타있어서 제대로 불러지지 않으면 에러 남는다.

그래서 autoload가 필요하다



autoload는 사용자가 정의하지 않는 클래스를 만나면 자동으로 해당 파일을 불러 들이기 위해 사용합니다.

그래서 require_once를 제거 가능



이 autoload를 사용했다고 자동으로 클래스가 불러지는 건 아니고 autoload안에 불러올 내용은 우리가 직접 만들어 줘야 합니다.

```
spl_autoload_register('my_autoloader');
function my_autoloader($class)
{

    echo 'class :'.$class;
    require_once $class.'.php';

}
```

이렇게 하면되는데 autoload 부분을 다른 파일로 분리시켜서

autoload 파일을 삽입하는 방식으로 바꿔보자

vender 라는 폴더를 하나 만들고 그 아래 autoload.php 파일을 만들어 autoload 관련 익명 함수만 구현해 놓았습니다.



require_once 'vendor/autoload.php';

이렇게 한 줄에 끝낼 수 있다.



