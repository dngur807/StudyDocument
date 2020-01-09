#  ASP.NET과 Redis로 랭킹 서버 만들기 - 2

> 출처  http://lab.gamecodi.com/board/zboard.php?id=GAMECODILAB_Lecture_series&page=1&sn1=&divpage=1&sn=off&ss=on&sc=on&select_arrange=headnum&desc=asc&no=131 



#### 2.2 ASP.NET 프로젝트 구축

지금 부터 레디스와 통신할 서버 어플리케이션을 만들어 볼 것입니다.

ASP.NET을 활용하여 웹서버 방식으로 구축해보자!

#### 2.2.1 프로젝트 환경 구성

![2_1_REDIS](D:\Study\StudyDocs\StudyDocument\images\2_1_REDIS.PNG)

다음 화면에서 Web API를 선택합니다.

RESTful API 를 만들고 서비스 할 수 있는 프로젝트입니다.

일반적으로 모바일 게임에서 웹서버와 연동한다면 클라에서 uri 호출하는 방식을 사용하는데, WebAPI가 그런 기능에 딱 맞는 프로젝트 템플릿 이라 할 수 있다.

![2_2_REDIS](D:\Study\StudyDocs\StudyDocument\images\2_2_REDIS.PNG)

ASP.NET 을 사용하는 기본 웹 프레임 워크를 만들었습니다.

여기에 클라의 요청을 받는 부분과 레디스 명령어를 호출하는 로직을 넣을 것이다.

먼저 레디스 명령어를 호출하고 결과값을 받는 RedisManager 를 생성하자



첫 번째로 할 일은 레디스 클라이언트 라이브러리를 프로젝트에 추가하는 것이다.

우리는 닷넷프레임워크를 사용하고 있어서 닷넷용 레디스 클라이언트를 추가해 보겠습니다.

여러 가지 라이브러리가 있지만 stackexchange.redis를 사용하자

Nuget으로 추가

![2_3_REDIS](D:\Study\StudyDocs\StudyDocument\images\2_3_REDIS.PNG)

```
깃허브 주소 https://github.com/StackExchange/StackExchange.Redis
```

#### 2.2.2 레디스 매니저 작성

레디스와 연결할 클라 라이브러리 까지 준비 완료 이제 레디스 명령어들을 사용하여 랭킹에 필요한 기능들을 구현하자

총 세 가지 기능을 구현할 계획

1. 유저 점수 등록
2. 랭킹 리스트 가져온다
3. 유저의 순위를 가져온다

레디스 관련 기능은 RedisManager라는 클래스를 만들어서 구현할 것이다.

RedisManager.cs 파일을 생성 하자

레디스 커넥션을 생성합니다.

```
namespace MyRankingServer.Redis
{
    public class RedisManager
    {
        static ConnectionMultiplexer redis = ConnectionMultiplexer.Connect("localhost");

    }
}
```

유저의 점수를 등록하는 매소드는 zincrby 라는 명령어를 통해 구현

클라에서 SortedSetIncrement 라는 매소드가 zincrby명령어와 매칭되는 매소드이다.

```
 public static void add_score(string nickname, int score)
 {
 	IDatabase db = redis.GetDatabase();

    // 레디스 명령어 zincrby 에 해당하는 매소드를 호출
    // rank-bestscore 랭킹에 유저의 점수를 등록합니다.
    // 이미 등록되어 있다면 기존 점수에 추가합니다.
    db.SortedSetIncrement(redis_key, nickname, score);
}   
```

랭킹 리스트를 가져오는 부분입니다. zrevrange에 해당하는 매소드로 SortedSetRangeByRankWithScores를 호출합니다.

```
 // 상위 랭킹 리스트를 가져온다.
 public static List<ScoreInfo> get_top_rankings()
 {
 	IDatabase db = redis.GetDatabase();

    // 레디스 명령어 zrevrange 에 해당하는 매소드를 호출합니다.
    SortedSetEntry[] list = db.SortedSetRangeByRankWithScores(redis_key, 0, 9, Order.Descending);
    List<ScoreInfo> ranks = new List<ScoreInfo>();
    for (int i = 0; i < list.Length; ++i)
    {
    	ranks.Add(new ScoreInfo { nickname = list[i].Element, score = (int)list[i].Score });
    }
return ranks;
}
```



마지막으로 유저의 순위를 가져오는 기능힙니다.

레디스 명령어 zrank에 해당하는 매소드는 SortedSetRank입니다.

```

// 유저의 순위를 가져온다
// 순위 없으면 0 리턴
public static long get_user_rank(string nickname)
{
    IDatabase db = redis.GetDatabase();
    // 레디스 명령어 zrank에 해당하는 매소드를 호출합니다.
    // Order.Descending 파라미터를 통해 가장 높은 점수가 1등으로 처리되도록 합니다.
    long? rank = db.SortedSetRank(redis_key, nickname, Order.Descending);

    // 정상적인 값이 들어있으면 순위를 리턴
    if (rank.HasValue)
    {
   	 	return rank.Value;
    }

    // 만약 아직 등록되지 않은 유저의 점수를 요청한 경우면 -1을 리턴
    return -1;
}
```

랭킹 정보를 담을 ScoreInfo클래스는 Models 폴더에 ScoreInfo.cs 라는 파일명으로 생성해 줍니다.

```
namespace MyRankingServer.Models
{
    public class ScoreInfo
    {
        public string nickname { get; set; }
        public int score { get; set; }
    }
}
```

이 클래스는 랭킹 리스트를 가져올 뿐만 아니라 클라 점수를 등록을 요청받을 때도 사용

#### 2.2.3 클라 요청 처리하기 - RankingController 생성

레디스 매니저를 통해서 레디스와 통신할 준비는 끝났습니다. 이제 클라부터 적절한 API를 요청받아 처리하는 부분을 작성해 보자

```
ASP.NET MVC 프로젝트 환경에서는 클라 요청을 받아들이는 부분을 컨트롤러 라고 한다. 
MVC 에서 M = 모델 , V = 뷰 , C = 컨트롤러를 의미합니다.
모델은 데이터가 정의 되어 있는 영역
뷰는 클라 화면에 보여지는 부분
컨트롤러는 요청을 받아 처리하는 부분을 뜻한다.
이 프로젝트에서는 API 요청을 받아 결과값을 문자열로 응답해주는 로직만 있으면 되기 때문에 뷰에 관한 코딩은 구현하지 않을 것입니다.
```

Controllers 폴더에서 마우스 우클릭 - 추가 - 컨트롤러를 선택합니다.

Web API 2 컨트롤러 - 비어있음을 선택하고 컨트롤러 이름은 RankingsController 라고 입력하자

![2_4_REDIS](D:\Study\StudyDocs\StudyDocument\images\2_4_REDIS.PNG)

유저의 요청도 세 가지 기능으로 정의할 수 있다.

첫째 랭킹 리스트를 가져온다

둘째 유저의 순위 가져온다

셋째 유저의 점수를 등록

각각의 기능들은 앞서 구현했던 레디스 매니저의 기능과 1:1 로 매칭 됩니다.



랭킹 리스트를 요청하는 부분입니다.

Get 방식으로 요청

```
// 상위 랭킹을 가져옵니다.
public IEnumerable<ScoreInfo> GetTopRankings()
{
	return RedisManager.get_top_rankings();
}
```

점수 등록 요청 부분

Get방식으로 요청하는데 뒤에 nickname 파라미터를 붙여 줍니다.

```
// 유저의 순위를 가져옵니다.
// 호출 방식 : /api/rankings?nickname={nickname}
public long GetUserRank(string nickname)
{
	return RedisManager.get_user_rank(nickname);
}
```

점수를 등록 요청 부분

점수 등록은 post 방식으로 form 데이터를 받아 처리하도록 하였다

form 데이터 항목들은 ScoreInfo 클래스에 들어 있는 프로퍼티들과 매칭되어 작동

```
/// <summary>  
/// 점수를 등록한다.  
/// </summary>  
/// <param name="scoreinfo"></param>  
/// <returns></returns>  
[HttpPost]  
public IHttpActionResult Post(ScoreInfo scoreinfo)  
{  
    RedisManager.add_score(scoreinfo.nickname, scoreinfo.score);  
    return Ok();  
}  
```

여기까지 유저의 요청을 처리하는 컨트롤러 구현했다.

온라인 게임 서버를 개발하다가 웹서버 로직 보면 소켓을 생성하고 리슨하는 부분이 어딘지 궁금할 텐데

해당 부분은 IIS라고 불리우는 웹서버에 이미 구현되어있다. 따라서 클라 요청이 앱으로 넘어온 후부터 로직만 작성하면 된다.





