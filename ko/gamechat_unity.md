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
|

### 2. Connect to GameChat Server

- 유저아이디를 통해, GAMECHAT 소켓 서버에 접속합니다. 

    => GAMECHAT 프로젝트 내에서, 유저아이디는 Unique 한 값입니다. 

- api를 사용하기 위한 토큰값을 획득합니다.

   => (connect 성공 이후 시점) GameChat.setting.Token 토큰값이 갱신됩니다.

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
| USER_ID | string | 유저 아이디 |
|

### 3. Disconnect from GameChat Server

 - 연결된 GAMECHAT 소켓서버와의 연결을 해제합니다.

```csharp
GameChat.disconnect();
```

### 4. Update User Profile

 - 유저정보가 저장/갱신됩니다.

```csharp
// (connect 성공 이후 시점) 갱신됩니다.
GameChatSetting GameChat.setting;

GameChatSetting
{
   public string Adid = "";
   public string MemberId = "";
   public string NickName = "";
   public string ProfileUrl = ""; // image profile url
   public string Token = "";
}
```

| ID         | type   | desc                          |
| :--------- | :----- | :---------------------------- |
| Adid       | string | 광고아이디(Unique identifier) |
| MemberId   | string | 유저 아이디                   |
| NickName   | string | 유저 닉네임                   |
| ProfileUrl | string | 프로필 이미지 url             |
| Token      | string | Authentification Token        |
|

```csharp
// (initialize 성공 이후 시점) 갱신됩니다.
GameChatDeviceInfo GameChat.deviceInfo;

GameChatDeviceInfo
{
    public string AppVersion = "";
    public string AppVersionCode = "";
    public string DeviceModel = "";
    public string DeviceModelVersion = "";
    public string DeviceOSVersion = "";
    public string NetworkType = "";
}
```

| ID         | type   | desc                          |
| :--------- | :----- | :---------------------------- |
| AppVersion       | string |  |
| AppVersionCode   | string |            |
| DeviceModel   | string |                 |
| DeviceModelVersion | string |           |
| DeviceOSVersion      | string |        |
| NetworkType      | string |       |
|

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
|

### 2. SendMessage

- 채널 아이디로, 특정 채널에 메시지를 송신합니다.

```csharp
GameChat.sendMessage(CHANNEL_ID, MESSAGE);
```

| ID         | type   | desc               |
| :--------- | :----- | :----------------- |
| CHANNEL_ID | string | 채널 아이디        |
| MESSAGE    | string | 전송 메시지 텍스트 |
|

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

public delegate void onEventReceivedCallback(string payload);
public onEventReceivedCallback onEventReceived;
//'event' Event에 대한, callback

public delegate void onErrorReceivedCallback(string payload);
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
    // 네트웍 연결 오류 및 타임아웃 발생 시
    public static readonly int CODE_SERVER_NETWORK_ERROR    = 4002;
     // 서버에서 받은 데이터를 파싱할 때 오류
    public static readonly int CODE_SERVER_PARSING_ERROR    = 4003;

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
| user_id    | string | 유저 아이디   |
| created_at | string | 생성 일자     |
|

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
}));
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
    public int    join_count;
    public string created_at;
    public string updated_at;
    public bool   is_private = false;

}
```

| ID         | type   | desc                         |
| :--------- | :----- | :--------------------------- |
| id         | string | 채널 아이디(unique)     |
| project_id | string | 프로젝트 아이디              |
| unique_id | string |  개발사에서 설정 가능한 채널 아이디(unique) |
| name       | string | 채널 이름                  |
| user_id       | string | (채널 생성한) 유저 아이디    |
| join_count | int    | 유저 수                      |
| is_private | bool    | 공개 여부                     |
| created_at | string | 생성 일자                    |
| updated_at | string | 갱신 일자                    |
|

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
| OFFSET     | int    | (가져올) 채널의 시작 위치 (index)    |
| LIMIT      | int    | (가져올) 채널의 수           |
|


### 2-3. getChannel

- (Channel) ID / UniqueID를 통해, Channel 데이터를 가져올 수 있습니다.

```csharp
//CHANNEL_ID로만 Search 할 경우, CHANNEL_UNIQUE_ID 파라메터에 null을 넣어주세요.

//CHANNEL_ID와 CHANNEL_UNIQUE_ID가 동시에 존재할 경우, CHANNEL_UNIQUE_ID 값을 우선으로 Search합니다.

GameChat.getChannel(CHANNEL_ID, CHANNEL_UNIQUE_ID, OFFSET, LIMIT, (Channel Channels, GameChatException Exception) => {

    if(Exception != null)
    {
        // Error 핸들링
        return;
    }

    //handling channelInfo instance
}));
```

```csharp
GameChat.getChannel(CHANNEL_UNIQUE_ID, OFFSET, LIMIT, (Channel Channels, GameChatException Exception) => {

    if(Exception != null)
    {
        // Error 핸들링
        return;
    }

    //handling channelInfo instance
}));
```


### 2-4. createChannel

 - (프로젝트 내) 새로운 Channel Instance를 생성할 수 있습니다.

```csharp

//CHANNEL_UNIQUE_ID는 (개발사에서 설정 가능한) 채널에 대한 unique value 입니다.

GameChat.createChannel(CHANNEL_NAME, CHANNEL_UNIQUE_ID, (Channel channel, GameChatException Exception) => {

    if(Exception != null)
    {
        // Error 핸들링
        return;
    }

    //handling created channel instance
}));
```

```csharp
GameChat.createChannel(CHANNEL_NAME, (Channel channel, GameChatException Exception) => {

    if(Exception != null)
    {
        // Error 핸들링
        return;
    }

    //handling created channel instance
}));
```

| ID         | type   | required | desc                         |
| :--------- | :----- | :----- |:--------------------------- |
| CHANNEL_NAME | string | required |(생성할) 채널 Name |
| CHANNEL_UNIQUE_ID | string | optional |(생성할) 채널 Unique ID |
|

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
}));
```

| ID         | type   | desc                         |
| :--------- | :----- | :--------------------------- |
| CHANNEL_ID | string | 채널 아이디 |
| CHANNEL_NAME | string | 채널 이름 |
|

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

}));
```

| ID         | type   | desc                         |
| :--------- | :----- | :--------------------------- |
| CHANNEL_ID | string | 채널 아이디 |
|

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
    }

    public class User
    {
        public string id;
        public string name;
        public string profile;
    }

    public string message_id;
    public string channel_id;
    public string sort_id;
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
| sort_id           |             | string  |                                 |
| message_type      |             | string  | 메세지 타입                     |
|                   | **content** | **Class** |                                 |
|                   | type        | string  | 메세지 타입                     |
|                   | lang        | string  | 메세지 언어                     |
|                   | text        | string  | 메세지 텍스트                   |
| mentions          |             | string  | 멘션(태그)                      |
| mentions_everyone |             | string  | 전체 메세지 여부                |
|                   | **sender**  | **Class**   |                                 
|                   | id          | string  | (송신한) 유저 아이디            |
|                   | name        | string  | (송신한) 유저 닉네임            |
|                   | profile     | string  | (송신한) 유저 이미지 프로필 url |
| created_at        |             | string  | 메세지 생성 일자                |

|

### 3-2. getMessages

 - (특정 채널에 대해) Message 데이터를 리스트 형태로 가져올 수 있습니다.

```csharp
GameChat.getMessages(CHANNEL_ID, OFFSET, LIMIT, SEARCH, QUERY, SORT, SORT_ID, (List<Message> Messages, GameChatException Exception) => {

    if(Exception != null)
    {
        // Error 핸들링
        return;
    }

    foreach(Message elem in Messages)
    {
        //handling each message instance
    }
}));
```

| ID         |     | type   | desc                        |
| :--------- | :-- | :----- | :-------------------------- |
| CHANNEL_ID |     | string | 채널 아이디                 |
| OFFSET     |     | string | (가져올) 메세지의 시작 위치 |
| LIMIT      |     | string | (가져올) 메세지의 갯수      |
| SEARCH     |     | string |                             |
| QUERY      |     | string |                             |
| SORT       |     | string |  메시지 리스트 정렬 순(asc/desc )                      |
| SORT_ID    |     | string |                             |
|


### 4-1. Member

 - (Received) Member Data Class (per Unit)

```csharp
public class Member
{
    public string id = "";
    public string project_id = "";
    public string machine_id = "";
    public string nickname = "";
    public string profile_url = "";
    public string memo = "";
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
| id                            | string  |             |
| project_id              | string  |                      |
| machine_id            | string  |                                 |
| nickname             | string  |                     |
| profile_url             | string  |                     |
| memo             | string  |                     |
| country             | string  |                     |
| remoteip             | string  |                     |
| adid             | string  |                     |
| device             | string  |                     |
| network             | string  |                     |
| version             | string  |                     |
| model             | string  |                     |
| logined_at             | string  |                     |
| created_at             | string  |                     |
| updated_at             | string  |                     |
|

### 4-2. updateMember

 - 서버의 유저 정보를 갱신할 수 있습니다.

```csharp
GameChat.updateMember(MEMBER_ID, NICKNAME, PROFILE, MEMO, ADID, DEVICE, NETWORK, VERSION, MODEL, (Member member, GameChatException Exception) => {

    if(Exception != null)
    {
        // Error 핸들링
        return;
    }

    //handling updated Member instance
}));
```

| ID         |     | type   | desc                        |
| :--------- | :-- | :----- | :-------------------------- |
| MEMBER_ID |     | string | 멤버 아이디                 |
| NICKNAME     |     | string | 닉네임 |
| PROFILE      |     | string | 프로필 이미지 url      |
| MEMO     |     | string |                             |
| ADID      |     | string |  광고식별자              |
| DEVICE       |     | string |                             |
| NETWORK    |     | string |                             |
| VERSION    |     | string |                             |
| MODEL    |     | string |                             |
|
