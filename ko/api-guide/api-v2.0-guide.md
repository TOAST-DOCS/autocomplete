<!-- pre-align:aligned sig=f2a39712547d -->

<a id="search-autocomplete-api-v20-guide"></a>
## Search > Autocomplete > API v2.0 가이드 { #search-autocomplete-api-v20-guide }

Cloud Search에서 제공하는 Autocomplete API v2.0을 설명합니다.

<a id="common"></a>
## 공통 { #common }

<a id="api-endpoint"></a>
### API 엔드포인트 { #api-endpoint }

<a id="api-endpoint-uri-information"></a>
#### URI 정보

| 환경 | URI                                              |
| ---- | ------------------------------------------------ |
| REAL | https://kr1-autocomplete.api.nhncloudservice.com |

<a id="api-endpoint-path-parameter-information"></a>
#### Path 파라미터 정보

| 이름      | 설명                    |
| --------- | ----------------------- |
| appKey    | 콘솔에서 발급 받은 앱키 |
| serviceId | 사용자의 임의의 이름    |

<a id="authentication-and-authorization"></a>
### 인증 및 권한 { #authentication-and-authorization }

Autocomplete API를 사용하려면 Appkey가 필요합니다. Appkey는 API 호출 시 요청 URL에 포함하여 특정 리소스를 가리키고 식별하는 데 사용됩니다.
Appkey 확인 및 사용에 대한 자세한 내용은 [Appkey](/nhncloud/ko/public-api/appkey)를 참고하세요.

<a id="full-indexing"></a>
## 전체 색인 { #full-indexing }

전체 색인을 실행하면 기존에 색인했던 파일은 사라집니다.

반드시 시작-색인-끝의 순서로 진행해야 합니다.

<a id="start"></a>
### 1. 시작 { #start }

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

<a id="indexing"></a>
### 2. 색인 { #indexing }

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

<a id="end"></a>
### 3. 끝 { #end }

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

<a id="cancel"></a>
### 4. 취소 { #cancel }

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

<a id="update-the-index"></a>
## 색인 업데이트 { #update-the-index }

색인을 업데이트하려면 id가 반드시 필요합니다.

add는 기존에 문서가 존재하면 수정, 존재하지 않으면 추가됩니다.

delete는 해당 문서를 삭제합니다.

<a id="update-the-index-2"></a>
### 1. 색인 업데이트 { #update-the-index-2 }

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

<a id="index-log"></a>
## 색인 로그 { #index-log }

<a id="view-the-index-log"></a>
### 1. 색인 로그 조회 { #view-the-index-log }

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

<a id="search"></a>
## 검색 { #search }

input을 검색할 수 있습니다.

<a id="search-2"></a>
### 1. 검색 { #search-2 }

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
