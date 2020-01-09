# 인터페이스

인터페이스는 

상속 받은 클래스에게 무조건 함수나  프로퍼티를 만들 도록 강요

상대방 타입을 한방에 다를 ㅅ ㅜ있음 공통된 기능을 타입 식별할 필요없이 사용가능



인터페이스 구현 강력 몬스터 ,아이템, 플레이어 공통적인 기능을 한방에 처리



인터페이스 없으면 어떻게 하는지 보자



미리 player 스크립트를 만들었다



hp, gold ,Rigidbody를 통해 플레이어 제어 



이것을 보진않겠다 이건 핵심 아니니깐



중요한건 플레이어 아이템얻을 수 있는데 



인터페이스 없으면 모가 불편한지 보자!



아이템을 통해 hp , 골드를 증가해보자



스프라이트에 2D 이미지 사용 골드 배치하고 



AddComponent Circle Collider 2D로 충돌체크핮



에일리언은 Capsule Collider가있으니



충돌 감지되면 골드 먹을 수 있겠지.!

트리거 켜서 충돌처리되고 뚫고 지나갈 수 있게 해주자!



골드 아이템 스크립트를 만들자

GoldItem으로 만들자

플레이어가 골드를 먹었을 때 골드값을 증가 시키자



public int goldAmount = 100f;

public void Use()

{

​	// 플레이어 골드 증가

​	Debug.Log("골드를 먹었다!");

​	Player player = FindObjectOfType<Player>(); //씬상에 존재하는 플레이어를 가져오자

​	player.gold += goldAmount;

​	gameObject.SetActive(false);// 먹혀졌으면 안보이게 하자

}



이제 이 스크립트를 붙여주자

이제 플레이어 스크립트에서 OnTriggerEnter를 알 고 있다.

이것은 유니티가 자동으로 발동 충돌한 상대방 들어오지,

2D게임이라 2D가 들어오는 차이점이 있음

void OnTriggerEnter2D(Collider2D other)

{

​	GoldItem goldItem = other.GetComponent<GoldItem>();



​	if (goldItem != null)

​	{

​		goldItem.Use();// public 이기 때문에 사용된다.		

​	}

}

골드를 먹어보자 1100으로 증가하고 로그가 찍힌다

문제는 아이템 종류가 한가지가 아닐경우



healthkit 아이템이 있다고 하자

똑같은 방법으로 처리해보자 isTrigger 체크해서 감지할 수 있도록 하자

스크립트 만들자

public int restoreHealth = 50;

public void Use()

{

​	플레이어 가져와서 

​	가져온 플레이어 체력 충전하고 꺼주자

}



player 스크립트에서 Healthkit인지 확인해야합



밑에다 코드를 또 작성해야되지...



문제는 아이템종류가 한두가지가 아니지



인터페이스를 만들자

IItem  모든 코드 지우고

public interface IItem

 함수 하나만 만들자 Use 

내부는 구현하지 않아야한다. 껍데기 역할



이제 아이템에  MonoBehaviour, IItem를 상속받자

인터페이스 상속받는 친구는 정의한 메소드를 무조건 만들어야 한다.

ㅇ없으면 에러 발생



인터페이스 1역할은 강제한다는 것이다.

내부에서 어떤 동작을 할지 정의하지 않는다.



왜 강력한지 보자!

Item에 모두 상속하자



인터페이스 강력한 점은 너가 무슨타입인지 관심없고 Use매소드가 있는건만 관심있음



이제 OnTrigger에서

IItem을 가져올 수 있니??

골드, 헬스킷인지 관심없어서 

item.Use(); 



즉 인터페이스 강력한 점은 타입을 체크할 필요없어!!



아이템 구별할 필요없다는 것이다.



아이템 말고도 직업 , 몬스터 등등에 사용할 수 있다.!!