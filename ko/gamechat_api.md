---
search:
  keyword:
    - gamepot
---

# Open API

Game Chat의 몇몇 기능을 규정 된 API로 호출할 수 있는 기능입니다.
> 대시보드에서 발급한 허용된 API Key를 사용해야 호출이 가능합니다.

## API Key 확인

API Key는 <b>대시보드 &gt; 설정 &gt; 프로젝트 설정 &gt; API Key</b> 에서 생성할 수 있습니다.

> 재발급 버튼을 누르면 키가 재발급 되며, 이전 키는 사용할 수 없으니 주의하세요.

## Open API 사용하기

### Base URL

```curl
https://dashboard-api.gamechat.naverncp.com/v1/api/project/{projectId}

- {projectId}부분에는 Game Chat의 project id를 적용
```



### 공통 Error code

Open API 요청시 발생하는 공통 에러코드입니다.

| Code | Description                                       |
| :--- | :------------------------------------------------ |
| -1   | 대시보드에 없는 키를 사용한 경우                          |
| -2   | 대시보드의 키와 헤더의 키가 다른경우                       |
| -3   | 대시보드에서 삭제한 키를 사용한 경우                       |
| -4   | 대시보드에서 미사용으로 처리된 키를 사용한 경우               |
| -5   | 키가 만료된 경우                                      |
| -6   | 프로젝트 아이디가 없는 경우                              |

### 채널 생성 API

채널을 생성합니다.

#### Request

 - Method : POST
 - URI : /channel

```text
POST
url : https://dashboard-api.gamechat.naverncp.com/v1/api/project/{projectId}/channel
Header : 'x-api-key: {API Key}'
Header : 'content-type: application/json'
data: '{
    "name":"#All"
}'
```
| Header    | Type   | Required | Description                  |
| :-------- | :----- | :------- | :--------------------------- |
| x-api-key | String | O        | 대시보드 > 설정 > 프로젝트 설정 > API Key |

| Attribute   | Type    | Required | Description                                                  |
| :---------- | :------ | -------- | :----------------------------------------------------------- |
| projectId   | String  | O        | 프로젝트 아이디 (대시보드 > 설정 > 프로젝트 설정 > 프로젝트 ID) |
| name        | String  | O        | 채널 명                                                      |
| translation | Boolean | X        | 번역 가능 여부                                               |
| uniqueId    | String  | X        | 임의로 지정할 수 있는 고유 아이디                            |

#### Response

```javascript
{
    "status": 1,
    "result": "1a51af0d-f464-4440-8ad6-ce69e0bdaba8"
}
```

| Attribute | Type   | Description                                |
| :-------- | :----- | :----------------------------------------- |
| status    | Int    | 결과값 \(1: 성공, 실패는 Error code 참고\) |
| result    | String | 생성된 채널 아이디                         |

#### Error code

| Code | Description               |
| :--- | :------------------------ |
| -100 | 필수 파라미터가 없는 경우 |

### 채널 수정 API

채널의 정보를 수정합니다.

#### Request

 - Method : PUT
 - URI : /channel/{channelId}

```text
PUT
url : https://dashboard-api.gamechat.naverncp.com/v1/api/project/{projectId}/channel/{channelId}
Header : 'x-api-key: {API Key}'
data: '{
    "name":"#GUILD"
}'
```
| Header    | Type   | Required | Description                  |
| :-------- | :----- | :------- | :--------------------------- |
| x-api-key | String | O        | 대시보드 > 설정 > 프로젝트 설정 > API Key |

| Attribute  | Type    | Required | Description                                                  |
| :--------- | :------ | -------- | :----------------------------------------------------------- |
| projectId  | String  | O        | 프로젝트 아이디 (대시보드 > 설정 > 프로젝트 설정 > 프로젝트 ID) |
| channelId  | String  | O        | 채널 아이디                                                  |
| name       | String  | X        | 채널 명                                                      |
| traslation | Boolean | X        | 번역 가능 여부                                               |

#### Response

성공

```javascript
{
    "status": 1,
    "message": "success"
}
```

| Attribute | Type   | Description                                |
| :-------- | :----- | :----------------------------------------- |
| status    | Int    | 결과값 \(1: 성공, 실패는 Error code 참고\) |
| message   | String | 결과 메시지                                |

### 채널 삭제 API

채널을 삭제합니다.

#### Request

 - Method : DELETE
 - URI : /channel/{channelId}

```text
DELETE
url : https://dashboard-api.gamechat.naverncp.com/v1/api/project/{projectId}/channel/{channelId}
Header : 'x-api-key: {API Key}'
```

| Header    | Type   | Required | Description                               |
| :-------- | :----- | :------- | :---------------------------------------- |
| x-api-key | String | O        | 대시보드 > 설정 > 프로젝트 설정 > API Key |

| Attribute | Type   | Required | Description                                                  |
| :-------- | :----- | -------- | :----------------------------------------------------------- |
| projectId | String | O        | 프로젝트 아이디 (대시보드 > 설정 > 프로젝트 설정 > 프로젝트 ID) |
| channelId | String | O        | 채널 아이디                                                  |

#### Response

성공

```javascript
{
    "status": 1,
    "message": "success"
}
```

| Attribute | Type   | Description                                |
| :-------- | :----- | :----------------------------------------- |
| status    | Int    | 결과값 \(1: 성공, 실패는 Error code 참고\) |
| message   | String | 결과 메시지                                |
