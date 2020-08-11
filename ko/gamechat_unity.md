---
search:
  keyword: ["gamechat"]
---

# GameChat Unity SDK

## Authentification

### 1. Initializing with Project ID

생성된 GameChat 프로젝트 아이디를 통해, GameChat 인스턴스를 초기화합니다.

```csharp
GameChat.initialize(PROJECT_ID);
```

| ID         | type   | desc            |
| :--------- | :----- | :-------------- |
| PROJECT_ID | string | 프로젝트 아이디 |
|

### 2. Connect to GameChat Server

-  유저아이디를 통해, 게임챗 소켓 서버에 접속합니다.

-  api를 사용하기 위한 토큰값을 획득합니다.
   
    (connect 성공 이후,) CoreManager.token에서 토큰값 확인 가능

```csharp
GameChat.connect(USER_ID, (string err, string user_id)=> {

    if(err != null)
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

연결된 GameChat 소켓서버와의 연결을 해제합니다.

```csharp
GameChat.disconnect();
```

### 4. Update User Profile

유저정보를 업데이트 합니다.

```csharp
CoreManager.User
{
    public string id;
    public string name;
    public string profile;     //image profile url
}
```

| ID      | type   | desc              |
| :------ | :----- | :---------------- |
| id      | string | 유저 아이디       |
| name    | string | 유저 닉네임       |
| profile | string | 프로필 이미지 url |
|

## Communication

### 1. Subscribe / Unsubscribe

- 채널 아이디를 통해, 특정 채널에 (un)subscribe 합니다

```csharp
GameChat.subscribe(CHANNEL_ID);

GameChat.unsubscribe(CHANNEL_ID);
```

| ID         | type   | desc        |
| :--------- | :----- | :---------- |
| CHANNEL_ID | string | 채널 아이디 |
|

### 2. SendMessage

- 채널 아이디를 통해, 특정 채널에 메시지를 송신합니다.

```csharp
GameChat.sendMessage(CHANNEL_ID, MESSAGE);
```

| ID         | type   | desc        |
| :--------- | :----- | :---------- |
| CHANNEL_ID | string | 채널 아이디 |
| MESSAGE    | string | 전송 메시지 텍스트 |
|

## Event

### Binding Event

- 소켓서버로부터 수신하는 이벤트에 대해, 커스터마이징 핸들러를 등록할 수 있습니다,

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

## Client API

### 1-1. Subscription

(특정 채널에 입장한) 유저(Subscription) Data Class (per Unit)

```csharp
public class Subscription
{
    public string id;
    public string channel_id;
    public string user_id;
    public string created_at;
}
```

| ID         | type   | desc        |
| :--------- | :----- | :---------- |
| id | string |  유니크 아이디|
| channel_id | string |  채널 아이디|
| user_id    | string | 유저 아이디 |
| created_at | string | 생성 일자 |
|

### 1-2. getSubscriptions

(채널에 입장한) 유저 데이터를 리스트 형태로 가져올 수 있습니다.

```csharp
GameChat.getSubscriptions(CHANNEL_ID, OFFSET, LIMIT, (List<Subscription> subscriptions) => {
    
    foreach(Subscription elem in subscriptions)
    {
        //handling each subscription instance
    }
}));
```

### 2-1. Channel

Channel Data Class (per Unit)

```csharp
public class Channel
{
    public string id;
    public string project_id;
    public string name;
    public int join_count;
    public string created_at;
    public string updated_at;
}
```

| ID         | type   | desc        |
| :--------- | :----- | :---------- |
| id | string |  채널 아이디 (Base64 Encoded) |
| project_id | string |  프로젝트 아이디|
| name | string |  유저 닉네임|
| join_count | int |  유저 수 |
| created_at | string |  생성 일자 |
| updated_at    | string |  갱신 일자 |
|

### 2-2. getChannels

(프로젝트 내 생성된) Channel Instance를 리스트 형태로 가져올 수 있습니다.

```csharp
GameChat.getChannels(CHANNEL_ID, OFFSET, LIMIT, (List<Subscription> subscriptions) => {
    foreach(Subscription elem in subscriptions)
    {
        //handling each subscription instance
    }
}));
```

| ID         | type   | desc        |
| :--------- | :----- | :---------- |
| CHANNEL_ID | string |  채널 아이디 (Base64 Encoded)|
| OFFSET | int |  (가져올) 채널의 시작 위치|
| LIMIT | int |  (가져올) 채널의 수 |
|


### 3-1. Message

(Received) Message Data Class (per Unit)

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

| ID   |      | type   | desc        |
| :---------| :--------- | :----- | :---------- |
| message_id || string |  메시지 유니크 아이디 |
| channel_id | | string |  채널 아이디 |
| sort_id  || string |   |
| message_type || string | 메세지 타입|
| | **content** | Content |    |
| | type | string | 메세지 타입 |
| | lang | string | 메세지 언어 |
| | text | string | 메세지 텍스트 |
| mentions  || string |  멘션(태그) |
| mentions_everyone  || string | 전체 메세지 여부 |
| | **sender** | User |    |
| | id | string | (송신한) 유저 아이디 |
| | name| string | (송신한) 유저 닉네임 |
| | profile| string | (송신한) 유저 이미지 프로필 url |
| created_at  || string |  메세지 생성 일자 |
|

### 3-2. getMessages

(특정 채널의) Message Instance 를 리스트 형태로 가져올 수 있습니다.

```csharp
GameChat.getMessages(CHANNEL_ID, OFFSET, LIMIT, SEARCH, QUERY, SORT, SORT_ID, (List<Message> messages) => {
    foreach(Message elem in messages)
    {
        //handling each message instance
    }
}));
```

| ID   |      | type   | desc        |
| :---------| :--------- | :----- | :---------- |
| CHANNEL_ID || string | 채널 아이디 |
| OFFSET | | string | (가져올) 메세지의 시작 위치 |
| LIMIT  || string | (가져올) 메세지의 갯수 |
| SEARCH || string |  |
| QUERY || string |  |
| SORT || string |  |
| SORT_ID || string |  |
|