## Search > Autocomplete > API v2.0 가이드

NHN Cloud Search에서 제공하는 Autocomplete API v2.0 를 설명합니다.

## 공통

**[요청]**

URI 정보

| 환경 | URI                                              |
| ---- | ------------------------------------------------ |
| REAL | https://kr1-autocomplete.api.nhncloudservice.com |

Path 파라미터 정보

| 이름      | 설명                    |
| --------- | ----------------------- |
| appKey    | 콘솔에서 발급받은 앱 키 |
| serviceId | 사용자의 임의의 이름    |

## ZONE

### 1. 생성

**[요청]**

URI 정보

| 메서드 | URI                                       |
| ------ | ----------------------------------------- |
| POST   | /headquarter/v2.0/appkeys/{{appKey}}/zone |

**[응답]**

응답 본문

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

### 2. 조회

**[요청]**

URI 정보

| 메서드 | URI                                       |
| ------ | ----------------------------------------- |
| GET    | /headquarter/v2.0/appkeys/{{appKey}}/zone |

header 정보

| 이름      | 설명                  |
| --------- | --------------------- |
| X-NHN-uid | 콘솔에서 발급받은 UID |

**[응답]**

응답 본문

```
{
    "zone": "api-7ab1617e2df0f1d1"
}
```

### 3. 삭제

**[요청]**

URI 정보

| 메서드 | URI                                       |
| ------ | ----------------------------------------- |
| DELETE | /headquarter/v2.0/appkeys/{{appKey}}/zone |

**[응답]**

응답 본문

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

## 서비스

### 1. 생성

**[요청]**

URI 정보

| 메서드 | URI                                                     |
| ------ | ------------------------------------------------------- |
| POST   | /admin/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}} |

header 정보

| 이름      | 설명                  |
| --------- | --------------------- |
| X-NHN-uid | 콘솔에서 발급받은 UID |

BODY 정보

```
{
    "analysis": {
        "analyzer": {
            "jaso_analyzer": {
                "tokenizer" : "jaso_tokenizer"
            },
            "linguist2_analyzer": {
                "tokenizer" : "linguist2_tokenizer"
            }
        },
        "tokenizer": {
            "jaso_tokenizer": {
                "type": "jaso_tokenizer"
            },
            "linguist2_tokenizer" : {
                "type":"linguist2_tokenizer",
                "method":"sgmt",
                "hanaterm_options":"seq_loc inputenc=utf8 outputenc=utf8 +korea +japan +josacat +eomicat"
            }
        }
    },
    "number_of_shards": "3",
    "number_of_replicas": "1",
    "store": {
        "stats_refresh_interval": "-1"
    }
}
```

**[응답]**

응답 본문

```
{}
```

### 2. 조회

**[요청]**

URI 정보

| 메서드 | URI                                       |
| ------ | ----------------------------------------- |
| GET    | /admin/v2.0/appkeys/{{appKey}}/serviceids |

header 정보

| 이름      | 설명                  |
| --------- | --------------------- |
| X-NHN-uid | 콘솔에서 발급받은 UID |

**[응답]**

응답 본문

```
[ ]
```

### 3. 삭제

**[요청]**

URI 정보

| 메서드 | URI                                                     |
| ------ | ------------------------------------------------------- |
| DELETE | /admin/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}} |

header 정보

| 이름      | 설명                  |
| --------- | --------------------- |
| X-NHN-uid | 콘솔에서 발급받은 UID |

**[응답]**

응답 본문

```
{}
```

## 전체 색인

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

BODY 정보 (예시)

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

### 1. 색인 업데이트

**[요청]**

URI 정보

| 메서드 | URI                                                                 |
| ------ | ------------------------------------------------------------------- |
| POST   | /indexing/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/indexing |

BODY 정보 (예시)

```
[
    {
        "id": "id-1",
        "input": "나이키 신발",
        "weight": 1
    },
    {
        "id": "id-2",
        "input": "아디다스 신발",
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

URI 정보

| 메서드 | URI                                                                          |
| ------ | ---------------------------------------------------------------------------- |
| GET    | /indexing/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/indexing_log?id=1 |

파라미터 정보

| 이름 | 설명    |
| ---- | ------- |
| id   | 색인 ID |

**[응답]**

응답 본문 (예시)

```
{
    "request_time": "2024-02-15T17:18:56",
    "file_name": "payload-1.json",
    "file_size": 1210,
    "status": 4
}
```

### 2. 색인 로그 리스트 조회

**[요청]**

URI 정보

| 메서드 | URI                                                                                        |
| ------ | ------------------------------------------------------------------------------------------ |
| GET    | /indexing/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/indexing_log/list?from=0&size=1 |

파라미터 정보

| 이름 | 설명        |
| ---- | ----------- |
| from | 어디서 부터 |
| size | list의 크기 |

**[응답]**

응답 본문 (예시)

```
{
    "took": 1,
    "time_out": true,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": 1,
        "max_score": 1.0,
        "hits": [
            {
                "_index": "indexing_log",
                "_type": "indexing_log",
                "_id": "hAHiPsqq83jLo8Is^sangho-test-2",
                "_score": 1.0,
                "_source": {
                    "alias_name": "hAHiPsqq83jLo8Is^sangho-test",
                    "id": 1,
                    "file_name": "payload-1.json",
                    "file_size": 344,
                    "request_time": 1707985583000,
                    "status": 4,
                    "total_doc_count": 1,
                    "total_index_file_size": 161592
                }
            }
        ]
    }
}
```

## 검색

### 1. 검색

**[요청]**

URI 정보 (예시)

| 메서드 | URI                                                                                       |
| ------ | ----------------------------------------------------------------------------------------- |
| GET    | /indexing/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/autocomplete?count=10&query=신 |

파라미터 정보

| 이름  | 설명             |
| ----- | ---------------- |
| count | 결과 개수 (필수) |
| query | 쿼리 (필수)      |

**[응답]**

응답 본문 (예시)

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

## ACL

### 1. 조회

**[요청]**

URI 정보

| 메서드 | URI                                                         |
| ------ | ----------------------------------------------------------- |
| GET    | /admin/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/acl |

header 정보

| 이름      | 설명                  |
| --------- | --------------------- |
| X-NHN-uid | 콘솔에서 발급받은 UID |

**[응답]**

응답 본문

```
{
    "version": "0.0.1",
    "indexing": {
        "order": [
            "allow",
            "deny"
        ],
        "allow": "all"
    },
    "suggest": {
        "order": [
            "allow",
            "deny"
        ],
        "allow": "all"
    },
    "admin": {
        "order": [
            "allow",
            "deny"
        ],
        "allow": "all"
    },
    "log": {
        "order": [
            "allow",
            "deny"
        ],
        "allow": "all"
    }
}
```

### 2. 삽입

**[요청]**

URI 정보

| 메서드 | URI                                                         |
| ------ | ----------------------------------------------------------- |
| PUT    | /admin/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/acl |

BODY 정보

```
{
    "version": "0.0.1",
    "indexing": {
        "order": [
            "allow",
            "deny"
        ],
        "allow": "all",
        "deny" : [
            "127.0.0.1"
        ]
    },
    "suggest": {
        "order": [
            "allow",
            "deny"
        ],
        "allow": "all"
    },
    "admin": {
        "order": [
            "allow",
            "deny"
        ],
        "allow": "all"
    },
    "log": {
        "order": [
            "allow",
            "deny"
        ],
        "allow": "all"
    }
}
```

**[응답]**

응답 본문

```
{}
```
