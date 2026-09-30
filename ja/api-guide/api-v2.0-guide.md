<!-- pre-align:aligned sig=f2a39712547d -->

<a id="search-autocomplete-api-v20-guide"></a>
## Search > Autocomplete > API v2.0ガイド { #search-autocomplete-api-v20-guide }

Cloud Searchで提供するAutocomplete API v2.0を説明します。

<a id="common"></a>
## 共通 { #common }

<a id="api-endpoint"></a>
### APIエンドポイント { #api-endpoint }

<a id="api-endpoint-uri-information"></a>
#### URI情報

| 環境 | URI                                              |
| ---- | ------------------------------------------------ |
| REAL | https://kr1-autocomplete.api.nhncloudservice.com |

<a id="api-endpoint-path-parameter-information"></a>
#### Pathパラメータ情報

| 名前     | 説明                   |
| --------- | ----------------------- |
| appKey    | コンソールで発行されたアプリケーションキー |
| serviceId | ユーザーの任意の名前   |

<a id="authentication-and-authorization"></a>
### 認証及び権限 { #authentication-and-authorization }

Autocomplete APIを使用するには、Appkeyが必要です。Appkeyは、API呼び出し時にリクエストURLに含めて特定のリソースを指定し、識別するために使用されます。
Appkeyの確認及び使用に関する詳細は、[Appkey](/nhncloud/ja/public-api/appkey)を参照してください。

<a id="full-indexing"></a>
## 全体インデックス { #full-indexing }

全体インデックスを実行すると、以前にインデックスしたファイルは消えます。

必ず開始-インデックス-終了の順番で実行する必要があります。

<a id="start"></a>
### 1. 開始 { #start }

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

<a id="indexing"></a>
### 2. インデックス { #indexing }

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

<a id="end"></a>
### 3. 終了 { #end }

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

<a id="cancel"></a>
### 4. キャンセル { #cancel }

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

<a id="update-the-index"></a>
## インデックスのアップデート { #update-the-index }

インデックスをアップデートするにはidが必ず必要です。

addは既に文書が存在する場合は修正、存在しない場合は追加されます。

deleteは該当文書を削除します。

<a id="update-the-index-2"></a>
### 1. インデックスのアップデート { #update-the-index-2 }

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

<a id="index-log"></a>
## インデックスログ { #index-log }

<a id="view-the-index-log"></a>
### 1. インデックスログ照会 { #view-the-index-log }

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

<a id="search"></a>
## 検索 { #search }

inputを検索できます。

<a id="search-2"></a>
### 1. 検索 { #search-2 }

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
