## Search > Autocomplete > API v2.0 가이드

NHN Cloud Search에서 제공하는 Autocomplete API v2.0을 설명합니다.

## 공통

**[요청]**

URI 정보

| 환경 | URI                                              |
| ---- | ------------------------------------------------ |
| REAL | https://kr1-autocomplete.api.nhncloudservice.com |

Path 파라미터 정보

| 이름      | 설명                    |
| --------- | ----------------------- |
| appKey    | 콘솔에서 발급 받은 앱키 |
| serviceId | 사용자의 임의의 이름    |

## 전체 색인

전체 색인을 실행하면 기존에 색인했던 파일은 사라집니다.

반드시 시작-색인-끝의 순서로 진행해야 합니다.

### 1. 시작

**[요청]**

URI 정보

| 메서드 | URI                                                                            |
| ------ | ------------------------------------------------------------------------------ |
| POST   | /indexing/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/indexing/full/begin |

**[응답]**

응답 본문

```
{}
```

### 2. 색인

**[요청]**

URI 정보

| 메서드 | URI                                                                      |
| ------ | ------------------------------------------------------------------------ |
| POST   | /indexing/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/indexing/full |

BODY 정보(예시)

```
[
    {
        "id": "월간 업무 계획",
        "input": [
            "월간 업무 계획",
            "dnjfrks djqan rPghlr",
            "업무 계획",
            "djqan rPghlr",
            "계획",
            "rPghlr"
        ],
        "weight": 1,
        "output": "월간 업무 계획",
        "payload": [
            "{\"serviceType\":\"aa\",\"servicePlaceId\":\"bb\"}",
            "가나",
            "다라",
            "{\"serviceType\":\"cc\",\"servicePlaceId\":\"dd\"}"
        ]
    }
]
```

**[응답]**

응답 본문

```
{
    "id": 1
}
```

### 3. 끝

**[요청]**

URI 정보

| 메서드 | URI                                                                          |
| ------ | ---------------------------------------------------------------------------- |
| POST   | /indexing/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/indexing/full/end |

**[응답]**

응답 본문

```
{}
```

### 4. 취소

**[요청]**

URI 정보

| 메서드 | URI                                                                             |
| ------ | ------------------------------------------------------------------------------- |
| POST   | /indexing/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/indexing/full/cancel |

**[응답]**

응답 본문

```
{}
```

## 색인 업데이트

색인을 업데이트하려면 id가 반드시 필요합니다.

add는 기존에 문서가 존재하면 '수정'으로, 존재하지 않으면 '추가'로 동작합니다.

delete는 해당 문서를 삭제합니다.

### 1. 색인 업데이트

**[요청]**

URI 정보

| 메서드 | URI                                                                 |
| ------ | ------------------------------------------------------------------- |
| POST   | /indexing/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/indexing |

BODY 정보(예시)

```
[
    {
        "id": "id-1",
        "action": "add",
        "input": "나이키 운동화",
        "weight": 1
    },
    {
        "id": "id-2",
        "action": "delete"
    },
    {
        "id": "id-3",
        "action": "add",
        "input": "뉴발란스 운동화",
        "weight": 1
    }
]
```

**[응답]**

응답 본문

```
{
    "id": 2
}
```

## 색인 로그

### 1. 색인 로그 조회

**[요청]**

URI 정보(예시)

| 메서드 | URI                                                                          |
| ------ | ---------------------------------------------------------------------------- |
| GET    | /indexing/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/indexing_log?id=1 |

파라미터 정보

| 이름 | 설명    |
| ---- | ------- |
| id   | 색인 ID |

**[응답]**

응답 본문(예시)

```
{
    "request_time": "2024-02-15T17:18:56",
    "file_name": "payload-1.json",
    "file_size": 1210,
    "status": 4
}
```

## 검색

input을 검색할 수 있습니다.

### 1. 검색

**[요청]**

URI 정보(예시)

| 메서드 | URI                                                                                       |
| ------ | ----------------------------------------------------------------------------------------- |
| GET    | /indexing/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/autocomplete?count=10&query=신 |

파라미터 정보

| 이름  | 설명             |
| ----- | ---------------- |
| count | 결과 개수(필수) |
| query | 쿼리(필수)      |

**[응답]**

응답 본문(예시)

```
{
    "collections": [
        {
            "index": 0,
            "items": [
                [
                    "아디다스 신발"
                ],
                [
                    "나이키 신발"
                ]
            ],
            "title": ""
        }
    ],
    "query": [
        "신",
        "tls"
    ],
    "ver": "v2.0"
}
```
