## Search > Autocomplete > API v2.0ガイド

NHN Cloud Searchで提供するAutocomplete API v2.0を説明します。

## 共通

**[リクエスト]**

URI情報

| 環境 | URI                                              |
| ---- | ------------------------------------------------ |
| REAL | https://kr1-autocomplete.api.nhncloudservice.com |

Pathパラメータ情報

| 名前     | 説明                   |
| --------- | ----------------------- |
| appKey    | コンソールで発行されたアプリケーションキー |
| serviceId | ユーザーの任意の名前   |

## 全体インデックス

全体インデックスを実行すると、以前にインデックスしたファイルは消えます。

必ず開始-インデックス-終了の順番で実行する必要があります。

### 1. 開始

**[リクエスト]**

URI情報

| メソッド | URI                                                                            |
| ------ | ------------------------------------------------------------------------------ |
| POST   | /indexing/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/indexing/full/begin |

**[レスポンス]**

レスポンス本文

```
{}
```

### 2. インデックス

**[リクエスト]**

URI情報

| メソッド | URI                                                                      |
| ------ | ------------------------------------------------------------------------ |
| POST   | /indexing/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/indexing/full |

BODY情報(例)

```
[
    {
        "id": "月間業務計画",
        "input": [
            "月間業務計画",
            "dnjfrks djqan rPghlr",
            "業務計画",
            "djqan rPghlr",
            "計画",
            "rPghlr"
        ],
        "weight": 1,
        "output": "月間業務計画",
        "payload": [
            "{\"serviceType\":\"aa\",\"servicePlaceId\":\"bb\"}",
            "あい",
            "うえ",
            "{\"serviceType\":\"cc\",\"servicePlaceId\":\"dd\"}"
        ]
    }
]
```

**[レスポンス]**

レスポンス本文

```
{
    "id": 1
}
```

### 3. 終了

**[リクエスト]**

URI情報

| メソッド | URI                                                                          |
| ------ | ---------------------------------------------------------------------------- |
| POST   | /indexing/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/indexing/full/end |

**[レスポンス]**

レスポンス本文

```
{}
```

### 4. キャンセル

**[リクエスト]**

URI情報

| メソッド | URI                                                                             |
| ------ | ------------------------------------------------------------------------------- |
| POST   | /indexing/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/indexing/full/cancel |

**[レスポンス]**

レスポンス本文

```
{}
```

## インデックスのアップデート

インデックスをアップデートするにはidが必ず必要です。

addは既に文書が存在する場合は修正、存在しない場合は追加されます。

deleteは該当文書を削除します。

### 1. インデックスのアップデート

**[リクエスト]**

URI情報

| メソッド | URI                                                                 |
| ------ | ------------------------------------------------------------------- |
| POST   | /indexing/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/indexing |

BODY情報(例)

```
[
    {
        "id": "id-1",
        "action": "add",
        "input": "ナイキスニーカー",
        "weight": 1
    },
    {
        "id": "id-2",
        "action": "delete"
    },
    {
        "id": "id-3",
        "action": "add",
        "input": "ニューバランススニーカー",
        "weight": 1
    }
]
```

**[レスポンス]**

レスポンス本文

```
{
    "id": 2
}
```

## インデックスログ

### 1. インデックスログ照会

**[リクエスト]**

URI情報(例)

| メソッド | URI                                                                          |
| ------ | ---------------------------------------------------------------------------- |
| GET    | /indexing/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/indexing_log?id=1 |

パラメータ情報

| 名前 | 説明   |
| ---- | ------- |
| id   | インデックスID |

**[レスポンス]**

レスポンス本文(例)

```
{
    "request_time": "2024-02-15T17:18:56",
    "file_name": "payload-1.json",
    "file_size": 1210,
    "status": 4
}
```

## 検索

inputを検索できます。

### 1. 検索

**[リクエスト]**

URI情報(例)

| メソッド | URI                                                                                       |
| ------ | ----------------------------------------------------------------------------------------- |
| GET    | /indexing/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/autocomplete?count=10&query=く |

パラメータ情報

| 名前 | 説明            |
| ----- | ---------------- |
| count | 結果数(必須) |
| query | クエリ(必須)      |

**[レスポンス]**

レスポンス本文(例)

```
{
    "collections": [
        {
            "index": 0,
            "items": [
                [
                    "アディダス靴"
                ],
                [
                    "ナイキ靴"
                ]
            ],
            "title": ""
        }
    ],
    "query": [
        "く",
        "tls"
    ],
    "ver": "v2.0"
}
```
