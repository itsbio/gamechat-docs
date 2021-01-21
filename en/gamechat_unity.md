---
search:
  keyword: ["gamechat"]
---

# GameChat Unity SDK

## Authentification

### 1. Initializing with Project ID

 - 생성한 GAMECHAT 프로젝트 아이디를 통해, GAMECHAT 인스턴스를 초기화합니다.

```csharp
GameChat.initialize(PROJECT_ID);
```

| ID         | type   | desc            |
| :--------- | :----- | :-------------- |
| PROJECT_ID | string | 프로젝트 아이디 |


### 2. Connect to GameChat Server

- 유저아이디를 통해, GAMECHAT 소켓 서버에 접속합니다.

    => GAMECHAT 프로젝트 내에서, 유저아이디는 Unique 한 값입니다.

- api를 사용하기 위한 토큰값을 획득합니다.

   => (GameChat.connect 이후 시점) 갱신된 토큰값을 확인할 수 있습니다.

- (토큰값 획득과 함께) 현재 접속 디바이스에 대한 유저정보가 갱신됩니다.

    => GameChat.connect의 콜백으로 전달받는 Member는 갱신된 데이터입니다.

```csharp
GameChat.connect(USER_ID, (Member User, GameChatException Exception)=> {

    if(Exception != null)
    {
        // Error 핸들링
        return;
    }
});
```

| ID      | type   | desc        |
| :------ | :----- | :---------- |
| USER_ID | string | 유저 고유 아이디 |
|

### 3. Disconnect from GameChat Server

 - 연결된 GAMECHAT 소켓서버와의 연결을 해제합니다.

```csharp
GameChat.disconnect();
```

### 4. User Information

 - 유저정보가 저장/갱신됩니다.

```csharp
// (connect 이후 시점) 갱신됩니다.

string Adid = GameChat.getAdid();

string MemberId = GameChat.getMemberId();

string NickName = GameChat.getNickName();

string ProfileUrl = GameChat.getProfileUrl();

string Token = GameChat.getToken();
```

| ID         | type   | desc                          |
| :--------- | :----- | :---------------------------- |
| Adid       | string | 광고아이디(Unique identifier) |
| MemberId   | string | 유저 고유 아이디                   |
| NickName   | string | 유저 닉네임                   |
| ProfileUrl | string | 프로필 이미지 url             |
| Token      | string | Authentification Token        |
|

```csharp
// (initialize 이후 시점) 갱신됩니다.

GameChatDeviceInfo GameChat.getDeviceInfo();

GameChatDeviceInfo
{
    public string AppVersion = "";
    public string DeviceModel = "";
    public string DeviceOSVersion = "";
    public string NetworkType = "";
}
```

| ID         | type   | desc                          |
| :--------- | :----- | :---------------------------- |
| AppVersion       | string | 앱 버전(Edit > Project Settings > Player > Other Settings)|
| DeviceModel   | string | 접속 디바이스 모델 |
| DeviceOSVersion  | string | 접속 디바이스 환경 |
| NetworkType      | string |   접속 네트워크 타입 (CELLULAR, WIFI)  |


## Communication

### 1. Subscribe / Unsubscribe

- 채널 아이디로, 특정 채널에 (un)subscribe 합니다

```csharp
GameChat.subscribe(CHANNEL_ID);

GameChat.unsubscribe(CHANNEL_ID);
```

| ID         | type   | desc        |
| :--------- | :----- | :---------- |
| CHANNEL_ID | string | 채널 아이디 |


### 2. SendMessage

- 채널 아이디로, 특정 채널에 메시지를 송신합니다.

```csharp
GameChat.sendMessage(CHANNEL_ID, MESSAGE);
```

| ID         | type   | desc               |
| :--------- | :----- | :----------------- |
| CHANNEL_ID | string | 채널 아이디        |
| MESSAGE    | string | 전송 메시지 텍스트 |


## Event

### Binding Event

- GAMECHAT 소켓서버로부터 수신하는 이벤트에 대해, 커스텀 핸들러를 등록/해제 할 수 있습니다,

```csharp
GameChat.dispatcher.(EVENT_NAME) += (CALLBACK_FUNCTION);

GameChat.dispatcher.(EVENT_NAME) -= (CALLBACK_FUNCTION);
```

```csharp
public delegate void onConnectedCallback(string data);
public onConnectedCallback onConnected;
//'connect' Event에 대한, callback

public delegate void onDisconnectedCallback(string reason);
public onDisconnectedCallback onDisconnected;
//'disconnect' Event에 대한, callback

public delegate void onMessageReceivedCallback(Message message);
public onMessageReceivedCallback onMessageReceived;
//'message' Event에 대한, callback

public delegate void onErrorReceivedCallback(string result, GameChatException exception);
public onErrorReceivedCallback onErrorReceived;
//'error' Event에 대한, callback
```

## Exception

- GAMECHAT API 사용 중에 발생하는, Exception에 대한 공통 처리 Class 입니다.

```csharp

public class GameChatException
{
    // Detail Error Code

    // 알 수 없는 Error
    public static readonly int CODE_UNKNOWN_ERROR           = 0;
    // 초기화 실패
    public static readonly int CODE_NOT_INITALIZE           = 1;
    // 파라미터가 올바르지 않은 경우
    public static readonly int CODE_INVAILD_PARAM           = 2;
    // 소켓서버로부터 발생한 오류
    public static readonly int CODE_SOCKET_SERVER_ERROR     = 500;
    // 네트웍 연결 오류 및 타임아웃 발생 시
    public static readonly int CODE_SERVER_NETWORK_ERROR    = 4002;
    // 서버에서 받은 데이터를 파싱할 때 오류
    public static readonly int CODE_SERVER_PARSING_ERROR    = 4003;

    // HTTP 에러의 경우, 해당 상태코드가 응답코드로 전달됩니다. (400, 403 ...)

    // Error Code
    public int code { get; set; }
    // Error Message
    public string message { get; set; }
}

```

## Client API

### 1-1. Subscription

 - Subscription Data Class (per Unit)

```csharp
public class Subscription
{
    public string id;
    public string channel_id;
    public string user_id;
    public string created_at;
}
```

| ID         | type   | desc          |
| :--------- | :----- | :------------ |
| id         | string | 유니크 아이디 |
| channel_id | string | 채널 아이디   |
| user_id    | string | 유저 고유 아이디   |
| created_at | string | 생성 일자     |


### 1-2. getSubscriptions

 - (특정 채널에 대해) Subscription 데이터를 리스트 형태로 가져올 수 있습니다.

```csharp
GameChat.getSubscriptions(CHANNEL_ID, OFFSET, LIMIT, (List<Subscription> Subscriptions, GameChatException Exception) => {

    if(Exception != null)
    {
        // Error 핸들링
        return;
    }

    foreach(Subscription elem in Subscriptions)
    {
        //handling each subscription instance
    }
});
```

### 2-1. Channel

 - Channel Data Class (per Unit)

```csharp
public class Channel
{
    public string id;
    public string project_id;
    public string unique_id;
    public string name;
    public string user_id;
    public string created_at;
    public string updated_at;
}
```

| ID         | type   | desc                         |
| :--------- | :----- | :--------------------------- |
| id         | string | 채널 아이디(unique)     |
| project_id | string | 프로젝트 아이디              |
| unique_id | string |  개발사에서 설정 가능한 채널 아이디 (unique) |
| name       | string | 채널 이름                  |
| user_id     | string | (채널 생성한) 유저 아이디    |
| created_at | string | 생성 일자                    |
| updated_at | string | 갱신 일자                    |


### 2-2. getChannels

 - (프로젝트 내) Channel 데이터를 리스트 형태로 가져올 수 있습니다.

```csharp
GameChat.getChannels(CHANNEL_ID, OFFSET, LIMIT, (List<Channel> Channels, GameChatException Exception) => {

    if(Exception != null)
    {
        // Error 핸들링
        return;
    }

    foreach(Channel elem in Channels)
    {
        //handling each channelInfo instance
    }
}));
```

| ID         | type   | desc                         |
| :--------- | :----- | :--------------------------- |
| CHANNEL_ID | string | 채널 아이디 |
| OFFSET     | int    | (전체 채널 리스트로부터 가져올) 채널의 시작 위치 (index)    |
| LIMIT      | int    | (가져올) 채널의 갯수           |



### 2-3. getChannel

- (Channel) ID / UniqueID를 통해, Channel 데이터를 가져올 수 있습니다.

```csharp
//CHANNEL_ID로만 Search 할 경우, CHANNEL_UNIQUE_ID 파라메터에 null을 넣어주세요.

//CHANNEL_ID와 CHANNEL_UNIQUE_ID값이 함께 존재할 경우, CHANNEL_UNIQUE_ID 값을 우선으로 Search합니다.

GameChat.getChannel(CHANNEL_ID, CHANNEL_UNIQUE_ID, OFFSET, LIMIT, (Channel Channels, GameChatException Exception) => {

    if(Exception != null)
    {
        // Error 핸들링
        return;
    }

    //handling channelInfo instance
});
```

```csharp
GameChat.getChannel(CHANNEL_UNIQUE_ID, OFFSET, LIMIT, (Channel Channels, GameChatException Exception) => {

    if(Exception != null)
    {
        // Error 핸들링
        return;
    }

    //handling channelInfo instance
});
```


### 2-4. createChannel

 - (프로젝트 내) 새로운 Channel Instance를 생성할 수 있습니다.

```csharp

//CHANNEL_UNIQUE_ID는 (개발사에서 선택적으로 설정 가능한) 채널에 대한 unique value 입니다.

GameChat.createChannel(CHANNEL_NAME, CHANNEL_UNIQUE_ID, (Channel channel, GameChatException Exception) => {

    if(Exception != null)
    {
        // Error 핸들링
        return;
    }

    //handling created channel instance
});
```

```csharp
GameChat.createChannel(CHANNEL_NAME, (Channel channel, GameChatException Exception) => {

    if(Exception != null)
    {
        // Error 핸들링
        return;
    }

    //handling created channel instance
});
```

| ID         | type   | required | desc                         |
| :--------- | :----- | :----- |:--------------------------- |
| CHANNEL_NAME | string | required | (생성할) 채널 Name |
| CHANNEL_UNIQUE_ID | string | optional | (생성할) 채널 Unique ID |


### 2-5. updateChannel

 - (프로젝트 내) 기존 Channel Instance의 이름을 갱신할 수 있습니다.

```csharp
GameChat.updateChannel(CHANNEL_ID, CHANNEL_NAME, (JSONNode result, GameChatException Exception) => {

    if(Exception != null)
    {
        // Error 핸들링
        return;
    }

    //result => updated channel id
});
```

| ID         | type   | desc                         |
| :--------- | :----- | :--------------------------- |
| CHANNEL_ID | string | 채널 아이디 |
| CHANNEL_NAME | string | 채널 이름 |


### 2-6. deleteChannel

(프로젝트 내, 존재하는) Channel Instance를 삭제할 수 있습니다.

```csharp
GameChat.deleteChannel(CHANNEL_ID, (JSONNode result, GameChatException Exception) => {

    if(Exception != null)
    {
        // Error 핸들링
        return;
    }

    //result => deleted channel id,name

});
```

| ID         | type   | desc                         |
| :--------- | :----- | :--------------------------- |
| CHANNEL_ID | string | 채널 아이디 |


### 3-1. Message

 - (Received) Message Data Class (per Unit)

```csharp
public class Message
{
    public class Content
    {
        public string type;
        public string lang;
        public string text;
        public bool translated;
    }

    public class User
    {
        public string id;
        public string name;
        public string profile;
    }

    public string message_id;
    public string channel_id;
    public string message_type;
    public Content content;

    public string mentions;
    public bool mentions_everyone;
    public User sender;
    public string created_at;
}
```

| ID                |             | type    | desc                            |
| :---------------- | :---------- | :------ | :------------------------------ |
| message_id        |             | string  | 메시지 유니크 아이디            |
| channel_id        |             | string  | 채널 아이디                     |
| message_type      |             | string  | 메세지 타입                     |
|                   | **content** | **Class** |                             |
|                   | type        | string  | 메세지 타입                     |
|                   | lang        | string  | 메세지 언어                     |
|                   | text        | string  | 메세지 텍스트                   |
|                   | translated        | bool  | (자동) 텍스트 번역여부                  |
| mentions          |             | string  | 멘션(태그)                      |
| mentions_everyone |             | string  | 전체 메세지 여부                |
|                   | **sender**  | **Class** |                            |
|                   | id          | string  | (송신한) 유저 고유 아이디            |
|                   | name        | string  | (송신한) 유저 닉네임            |
|                   | profile     | string  | (송신한) 유저 이미지 프로필 url |
| created_at        |             | string  | 메세지 생성 일자                |


### 3-2. getMessages

 - (특정 채널에 대해) Message 데이터를 리스트 형태로 가져올 수 있습니다.

```csharp
GameChat.getMessages(CHANNEL_ID, OFFSET, LIMIT, SEARCH, QUERY, SORT, (List<Message> Messages, GameChatException Exception) => {

    if(Exception != null)
    {
        // Error 핸들링
        return;
    }

    foreach(Message elem in Messages)
    {
        //handling each message instance
    }
});
```

| ID         | type   | desc                        |
| :--------- | :----- | :-------------------------- |
| CHANNEL_ID | string | 채널 아이디                 |
| OFFSET     | string | (전체 메시지 리스트로부터 가져올) 메세지의 시작 위치 |
| LIMIT      | string | (가져올) 메세지의 갯수      |
| SEARCH     | string |   (메시지 검색 시) 검색 기준 key (ex> content.text) 빈 문자열 전달 시, full scan |
| QUERY      | string |  (메시지 검색 시) 검색 value. 완전 일치만 검색 가능. 빈 문자열 전달 시, full scan  |
| SORT       | string |  메시지 리스트 정렬순서 (default : desc - 가장 최근순) (optional : asc) |


### 3-3. translateMessage

 - (자동번역 기능이 활성화 되어 있을 경우) 임의의 텍스트를 (지정한 언어로) 번역할 수 있습니다.

```csharp
GameChat.translateMessage(CHANNEL_ID, SORCE_LANG, TARTGET_LANG, TEXT, (List<Message.Content> Messages, GameChatException Exception) => {

    if(Exception != null)
    {
        // Error 핸들링
        return;
    }

    foreach(Message.Content elem in Messages)
    {
        //handling each message instance
    }
});

GameChat.translateMessage(SORCE_LANG, TARTGET_LANG, TEXT, (List<Message.Content> Messages, GameChatException Exception) => {

    if(Exception != null)
    {
        // Error 핸들링
        return;
    }

    foreach(Message.Content elem in Messages)
    {
        //handling each message instance
    }
});
```

| ID         |     | type   | desc                        |
| :--------- | :-- | :----- | :-------------------------- |
| CHANNEL_ID |     | string | 채널 아이디                 |
| SORCE_LANG     |     | string | (송신 할) 텍스트 언어명 (auto : 자동감지) |
| TARTGET_LANG      |     | string | (번역 수신 할) 텍스트 언어명 ("," 구분하여 복수 입력 가능 - ex> "en, fr, th")      |
| TEXT     |     | string |     (송신 할) 텍스트                     |


### 4-1. Member

 - (Received) Member Data Class (per Unit)

```csharp
public class Member
{
    public string id = "";
    public string project_id = "";
    public string nickname = "";
    public string profile_url = "";
    public string country = "";
    public string remoteip = "";
    public string adid = "";
    public string device = "";
    public string network = "";
    public string version = "";
    public string model = "";
    public string logined_at = "";
    public string created_at = "";
    public string updated_at = "";
}
```

| ID                          | type    | desc                            |
| :----------------  | :------ | :------------------------------ |
| id                            | string  |  유저 고유 아이디          |
| project_id              | string  |    (로그인한) GameChat 프로젝트 아이디            |
| nickname             | string  |        유저 닉네임           |
| profile_url             | string  |      (이미지) 프로필 Url           |
| country             | string  |         접속 국가              |
| remoteip             | string  |     접속 IP             |
| adid             | string  |        광고 식별자            |
| device             | string  |          접속 디바이스 환경          |
| network             | string  |        접속 네트워크 타입(CELLULAR, WIFI)              |
| version             | string  |         접속 앱 버전          |
| model             | string  |     접속 디바이스 모델         |
| logined_at             | string  |   로그인한 일자           |
| created_at             | string  |   유저 생성 일자                  |
| updated_at             | string  |   유저 정보 갱신 일자            |


### 4-2. updateMember

 - 채팅 서버의 유저 정보를 갱신할 수 있습니다.

```csharp

//유저 닉네임 업데이트
GameChat.setNickname(MEMBER_ID, NICKNAME, (Member member, GameChatException Exception) => {

    if(Exception != null)
    {
        // Error 핸들링
        return;
    }
    //handling updated Member instance
});

//유저 프로필 이미지 url 업데이트
GameChat.setProfileUrl(MEMBER_ID, PROFILE, (Member member, GameChatException Exception) => {

    if(Exception != null)
    {
        // Error 핸들링
        return;
    }
    //handling updated Member instance
});
```

| ID         |  type   | desc                        |
| :--------- | :----- | :-------------------------- |
| MEMBER_ID  |  string |  유저 고유 아이디                 |
| NICKNAME   | string | 유저 닉네임 |
| PROFILE    | string | 프로필 이미지 url      |

