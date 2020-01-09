# override

오버라이드는 부모클래스에서 만든 것을 자식 클래스에 똑같이 만든것이다



BaseRotator 를 만들자

회전하는 객체는 모두 이것을 상속받아야한다.



public float speed = 60f;

void Update()

{

​	Rotate();

}

protected void Rotate()

{

// x축으로 1초에 스피드 만큼 회전

​	transform.Rotate()

}



3D object를 만들고 Base Rotate 붙이고 확인 

1초에 60도 회전하네



이제 xy 로테이터 , y 로테이터 등등

즉 알아서 각자 구현하게 하면 어떨까

어떻게 회전할 지는 부모에서 지정해야하나?? 회전만 하고 

어떻게 회전할지는 자식들에서 구현하면 어떨까 ??

BaseRotator 내부 구현을 자식들이 구현하게 하고 부모는 Rotate를 그냥 돌리면 된다.



virtual이라는 키워드가 있다.

자식들이 덮어쒸울 수 있다는 것이다.

가상이잖아  



이제 상속받은 x, y ,z 로테이터를 만들자



z rotate를 보자!



로테이트 상속받는데 virtual이라 오버라이드해서 덮어쒸우자



오버라이드는 함수에 형식이 같아야된다.

protected override void Rotate()

{

}

이러면 BaseRotate는 날아가고 Rotate로 대체된다.

부모에서 로테이트를 쓰면서 내껄 쓰고 싶으면

base.Rotate를 호출하면된다.



이거 함수를 하나만들어도 부모에서 update에서 Rotate를 그냥 호출하고 있다.

어떻게 되어 있던 상관없이 동작



x , y 로 각자 대체해보자!!

큐브를 두개 더 만들어서 대체해주자!

하나의 baseRotator를 상속받아서 

부모에서 만든기능을 자식이 커스터 마이징해서 쓸 수 있는 거임.!!!



