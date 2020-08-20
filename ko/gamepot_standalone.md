---
search:
  keyword: ["gamepot"]
---

# GamePot Unity SDK (Standalone)

***Standalone(Windows) 플랫폼을 위한 GamePot Unity SDK는 콘솔에서 웹뷰를 사용하기 위해, 3rd party Unity Asset 'VUPLEX(for Mac & Windows)' 를 사용하고 있습니다.***

***해당 Asset을  Asset Store에서 구입하여 import 한 다음, GamePot SDK를 사용해주세요.***

***GamePot Unity SDK v2.1.2를 기준으로 튜닝된 패키지 입니다.***

***[[VUPLEX Site Link]](https://developer.vuplex.com/webview/overview)***

***[[VUPLEX AssetStore Link]](hhttps://assetstore.unity.com/packages/tools/gui/3d-webview-for-windows-and-macos-154144)***


## Connect to Socket Server (for CCU Checking)

### Step 1

게임을 시작할때 로드되는 첫 장면에 사용되는 개체에 다음 코드를 추가합니다.

```csharp

using GamePotUnity;
public class GamePotLoginSampleScene : MonoBehaviour {
    void Awake() {

        // GamePot Project ID로, 초기화
        GamePot.initPlugin(GAMEPOT_PROJECT_ID);
    }
    void Start () {
        GamePot.setListener(  GamePotInterface.cs 상속받은 class );
         // ex) GamePot.setListener(new GamePotSampleListener());
    }
}

ex)
public class GamePotSampleListener : MonoBehaviour , IGamePot {
    ....
}
```

### Step 2

로그인 성공 시, 획득한 MemberID(GamePot) / Token(GamePot) 값을 GamePotSettings.MemberInfo에 저장합니다.

```csharp
// GamePot Unity SDK의 login api를 호출 하는 상황을 예시로 합니다.

GamePot.login(NCommon.LoginType.GUEST,
 (NCommon.ResultLogin resultState, NUserInfo userInfo, NAppStatus appStatus, NError erro=>
 {
     if (resultState == NCommon.ResultLogin.SUCCESS)
     {
         ...
            //로그인 성공 시, 전달받은 memberId / token 값을 GamePotSettings.MemberInfo에 넣어줍니다.
            ***************************************************
            [Step 2] - Set MemberInfo
            
            NUserInfo userInfo = new NUserInfo
            {
                 memberid = (string) GAMEPOT_MEMBER_ID,
                 token = (string) GAMEPOT_TOKEN
            };

            GamePotSettings.MemberInfo = userInfo;
            ***************************************************
          
            [Step 3] ....
        ...
     }
 });
```

| Struct                    | ID       | type   | desc             |
| :------------------------ | :------- | :----- | :--------------- |
| GamePotSetting.MemberInfo |          |        |                  |
|                           | memberid | string | 게임팟 Member ID |
|                           | token    | string | 게임팟 Token     |
|


### Step 3

동시접속자 체크를 위해, 소켓서버에 연결합니다.

```csharp
GamePotChat.setup(string store = "pc"); 
```

```csharp
// GamePot Unity SDK의 login api를 호출 하는 상황을 예시로 합니다.

            GamePot.login(NCommon.LoginType.GUEST,
            (NCommon.ResultLogin resultState, NUserInfo userInfo, NAppStatus appStatus, NError error) =>
            {
                if (resultState == NCommon.ResultLogin.SUCCESS)
                {
                   [Step 2] - Set MemberInfo ...

                    ...

                   ***************************************************
                    [Step 3] - Setup to Connect Socket Server
                    GamePotChat.setup(string store = "pc");
                   ***************************************************      
 
                    ...
                }
            });
}
```

### Step 4

(setup 이후,) 소켓 서버에 대해 핸들링(connect / disconnect) 할 수 있습니다.

```csharp
GamePotChat.start();    //connect
GamePotChat.stop();    //disconnect
```

## 공지사항 이미지 웹뷰

(GamePot SDK 초기화 이후,) 웹뷰 형태의 공지사항 이미지 팝업을 노출합니다.

```csharp
GamePot.showNoticeWebView();
```

- ref.  (Unity Editor 상에서) 노출되는 팝업 이미지의 레이아웃은 Sample Prefab으로 되어 있으며, 이를 수정해 레이아웃을 조정할 수 있습니다. (/Assets/Resources/GamePotWebViewManager)


## 고객지원 / FAQ 웹뷰

- ref. 현재 유니티 엔진 상에서, **웹뷰에 대한 Event 수신 / 한글 유니코드 입력이 불가한 이슈** 가 있습니다. 이에 따라, 아직 (공식적으로) 고객지원 메뉴에 대한 Native API는 제공되지 않는 상태입니다.

- ref. 참고 용도로 고객지원에 대한 웹뷰를 호출하는 코드를 공유드립니다. GET METHOD를 호출하는 방식으로 간단하게 웹뷰를 호출할 수있습니다.

```csharp
//Example - 대시보드 주소 및 각 파라메터 값을 생성하신 GamePot 대시보드에 대한 값으로 수정하여 접근이 가능합니다.
            GamePotWebViewManager.s_instance.showWebView("https://gsrpkjibrmls4086645.gcdn.ntruss.com/demo/cs/question?projectid=ab2775b4-cf09-4794-9480-decd607a7f8a&store=google&memberid=4e125b06-462f-4c9f-8dbe-b1447bc9e370&device=android&sdkversion=2.1.2&language=ko");
```

