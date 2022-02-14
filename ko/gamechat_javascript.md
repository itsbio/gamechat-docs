---
search:
  keyword: ['gamechat']
---

# Game Chat Javascript SDK

## Authentification

### 1. 대시보드에서 설정에서 프로젝트ID 를 확인 합니다.

[GameChat.min.js](https://kr.object.ncloudstorage.com/gamechat/gamechat.min.js) 을 다운로드 합니다.

<script src="GameChat.min.js"></script>

를 <head> </head> 사이에 추가 합니다.

게임챗을 사용하기전에 인스턴스를 초기화 해야 합니다. 대시보드에서 확인한 프로젝트ID 를 추가해 주세요.

```javascript
var gc = new gamechat.Chat();
gc.initialize(projectId);

// 싱가폴 리전 사용 시
gc.initialize(projectId, { region : 'sg' });
```

### 2. Connect to Game Chat Server

- 유저아이디를 통해, Game Chat 소켓 서버에 접속합니다.

```javascript
gc.setUser({
  id: 'userId',
  name: 'Nickname',
});
gc.connect(user_id, (err, res) => {
  if (err) console.log(err);
});
```

### 3. Disconnect from Game Chat Server

- 연결된 Game Chat 소켓서버와의 연결을 해제합니다.

```javascript
gc.disconnect();
```

## Communication

### 1. Subscribe / Unsubscribe

- 채널 아이디로, 특정 채널에 (un)subscribe 합니다

```javascript
gc.subscribe(CHANNEL_ID);
gc.unsubscribe(CHANNEL_ID);
```

| ID         | type   | desc        |
| :--------- | :----- | :---------- |
| CHANNEL_ID | string | 채널 아이디 |

### 2. SendMessage

- 채널 아이디로, 특정 채널에 메시지를 송신합니다.

```javascript
gc.sendMessage(CHANNEL_ID, MESSAGE);
```

| ID         | type   | desc               |
| :--------- | :----- | :----------------- |
| CHANNEL_ID | string | 채널 아이디        |
| MESSAGE    | string | 전송 메시지 텍스트 |

## Event

### Binding Event

- Game Chat 소켓서버로부터 수신하는 이벤트에 대해, 이벤트 핸들러를 등록/해제 할 수 있습니다,

```javascript
// 메시지 수신
gc.bind('onMessageReceived', function (channel, message) {});
// 오류 메시지
gc.bind('onErrorReceived', function (channel, message) {});
// 접속 성공
gc.bind('onConnected', function (channel, message) {});
// 접속 종료
gc.bind('onDisconnected', function (reason) {});
```

## Client API

### 1-1. Subscription

- Subscription Data Class (per Unit)

| ID         | type   | desc             |
| :--------- | :----- | :--------------- |
| id         | string | 유니크 아이디    |
| channel_id | string | 채널 아이디      |
| user_id    | string | 유저 고유 아이디 |
| created_at | string | 생성 일자        |

### 1-2. getSubscriptions

- (특정 채널에 대해) Subscription 데이터를 리스트 형태로 가져올 수 있습니다.

```javascript
gc.getSubscriptions(CHANNEL_ID, OFFSET, LIMIT, function(err, subscriptions)
{
    console.log(subscriptions);
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
    public string created_at;
    public string updated_at;
}
```

| ID         | type   | desc                                        |
| :--------- | :----- | :------------------------------------------ |
| id         | string | 채널 아이디(unique)                         |
| project_id | string | 프로젝트 아이디                             |
| unique_id  | string | 개발사에서 설정 가능한 채널 아이디 (unique) |
| name       | string | 채널 이름                                   |
| user_id    | string | (채널 생성한) 유저 아이디                   |
| created_at | string | 생성 일자                                   |
| updated_at | string | 갱신 일자                                   |

### 2-2. getChannels

- (프로젝트 내) Channel 데이터를 리스트 형태로 가져올 수 있습니다.

```csharp
GameChat.getChannels(OFFSET, LIMIT, function(err, channels) {
});
```

| ID     | type | desc                                                     |
| :----- | :--- | :------------------------------------------------------- |
| OFFSET | int  | (전체 채널 리스트로부터 가져올) 채널의 시작 위치 (index) |
| LIMIT  | int  | (가져올) 채널의 갯수                                     |

### 2-4. create / update / delete Channel

- (프로젝트 내) 새로운 Channel Instance를 생성 / 갱신 / 삭제할 수 있습니다.

> 보안상의 이슈로, SDK를 통한 채널의 CRUD 기능은 제거되었습니다.
> Open API를 통해, Server to Server로 채널의 CRUD를 사용하실 수 있습니다.

[ Guide => [ Open API - Channel Create / Update / Delete ]](https://docs.gamechat.kr/undefined/gamechat_api#api)

### 2-5. getMessages

- (특정 채널에 대해) Message 데이터를 리스트 형태로 가져올 수 있습니다.

```javascript
gc.getMessages( {channelId:CHANNEL_ID, offset:OFFSET, limit:LIMIT, search:SEARCH, query:QUERY, sort:SORT, sortId:SORTID},
  function (err, messages) {}
);
```

| ID         | type   | desc                                                                             |
| :--------- | :----- | :------------------------------------------------------------------------------- |
| CHANNEL_ID | string | 채널 아이디                                                                      |
| OFFSET     | int | (전체 메시지 리스트로부터 가져올) 메세지의 시작 위치                             |
| LIMIT      | int | (가져올) 메세지의 갯수                                                           |
| SEARCH     | string | (메시지 검색 시) 검색 기준 key (ex> content.text) 빈 문자열 전달 시, full scan   |
| QUERY      | string | (메시지 검색 시) 검색 value. 완전 일치만 검색 가능. 빈 문자열 전달 시, full scan |
| SORT       | string | 메시지 리스트 정렬순서 (default : desc - 가장 최근순) (optional : asc)           |
| SORTID     | string | 해당 sort id 다음으로 메시지를 가져온다. ( 이전 목록 보기 )                      |

### 3-3. translateMessage

- (자동번역 기능이 활성화 되어 있을 경우) 임의의 텍스트를 (지정한 언어로) 번역할 수 있습니다.

> 해당 기능은, NaverCloud PAPAGO NMT 상품을 함께 연동할 경우 사용 가능합니다. [[NCP Papago NMT]](https://www.ncloud.com/product/aiService/papagoTranslation)

- (Received) Translation Data Class (per Unit)

```javascript
const tr_message = gc.translateMessage(
  CHANNEL_ID,
  SORCE_LANG,
  TARTGET_LANG,
  MESSAGE
);
```
