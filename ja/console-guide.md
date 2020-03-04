## Search > Autocomplete > コンソール使用ガイド

## 注意

- 文書内のホスト名「api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com」は、ユーザーごとに異なる場合があります。
- 文書内のアプリケーションキー「7IkFjTvxA8zwfL8e」は、ユーザーごとに異なります。

## 始める

まずAutocompleteサービスを有効化します。
1. **TOAST Console**で**サービス選択**をクリックします。
2. **Autocomplete**をクリックします。
![img](https://camo.githubusercontent.com/fede2041ec8707d6f0c9d0adfe6445e968e67b09/687474703a2f2f7374617469632e746f6173746f76656e2e6e65742f70726f645f6175746f636f6d706c6574652f70726f647563742d7573652d30322d32303230303131372e313333372e706e67)

サービスが有効になっているかを確認する方法は次のとおりです。
1. **TOAST Console**左側のメニューで**Search**をクリックします。
2. **Autocomplete**が表示されていれば、サービスが有効になっているということです。
![img](https://camo.githubusercontent.com/02282e4d4bdf2cd04d07cba4fc856be137777c22/687474703a2f2f7374617469632e746f6173746f76656e2e6e65742f70726f645f6175746f636f6d706c6574652f70726f647563742d7573652d30332d32303230303131372e313430372e706e67)

## 基本使用方法

### 1. サービスの作成
1. **サービス作成**ボタンをクリックします。

2. **サービス作成**ウィンドウでサービスIDを入力します。

    - 英字小文字、数字およびアーダースコア(_)とハイフン(-)のみ使用できます。
    - 最初の文字に数字、アーダースコア(_)、ハイフン(-)は使用できません。
    - 2文字以上入力する必要があります。

3. **保存**ボタンをクリックします。

![img](https://camo.githubusercontent.com/6cdfb1dfdfb9da040ed77d952327091a38d130de/687474703a2f2f7374617469632e746f6173746f76656e2e6e65742f70726f645f6175746f636f6d706c6574652f646f6d61696e5f6372656174655f70726f6365647572652d32303230303131372e313431372e706e67)

作成されたサービス結果を確認します。  
1. 作成されたサービスID(test)をクリックします。

![img](https://camo.githubusercontent.com/b719ce4e9b8e37433c7451ec46ea1d1f3774b0dd/687474703a2f2f7374617469632e746f6173746f76656e2e6e65742f70726f645f6175746f636f6d706c6574652f646f6d61696e5f6372656174655f726573756c742d32303230303131372e313435392e706e67)

### 2. インデックス

インデックスするファイルを作成してインデックスする方法は次のとおりです。

**インデックスファイルの作成**

  - 下記例のような形式でインデックスリクエストファイルを作成します。
  - インデックスするファイルはUTF-8で作成する必要があります。
      - Windowsのメモ帳でファイルを保存する時は、エンコードをUTF-8に指定して保存します。
  - 例ではdata/documents.jsonという名前で作成しました。
  - 最大ファイルサイズは32MBです。

```
[
  {
    "input": "ナイキ",
    "weight": 3
  },
  {
    "input": "ナイキ運動靴",
    "weight": 2
  },
  {
    "input": "運動靴",
    "weight": 1
  }
]
```

**インデックス方法**

1. **インデックス**タブをクリックします。

2. **ファイル選択**ボタンをクリックします。

3. インデックスするファイルを選択します。

4. **開く**ボタンをクリックします。

5. インデックスコマンドがREST APIで出力されます。

6. **インデックス**ボタンをクリックします。

7. **更新**ボタンをクリックします。

8. インデックス結果を確認します。

![img](https://camo.githubusercontent.com/e5edbb5e02a0b3e89a688c599b767577e078bd45/687474703a2f2f7374617469632e746f6173746f76656e2e6e65742f70726f645f6175746f636f6d706c6574652f696e646578696e675f70726f6365647572652d30312d32303230303131372e313530382e706e67)  
![img](https://camo.githubusercontent.com/3552863677ff50b7c560d2f15ce020c026524cfb/687474703a2f2f7374617469632e746f6173746f76656e2e6e65742f70726f645f6175746f636f6d706c6574652f696e646578696e675f70726f6365647572652d30322d32303230303131372e313531362e706e67)

**インデックスの注意事項**

インデックスをリクエストすると、既存のデータはすべて削除され、新規データに変更されます。



**REST API**

  - インデックスAPI

    - Request

        - ファイルアップロード方式

            ```
            curl -XPOST 'http://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/indexing/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/test/indexing?split=true&koreng=true&chosung=false' -H 'Content-Type:multipart/form-data; charset=UTF-8' -F 'file=@documents.json'
            ```

        - Payload方式

            ```
            curl -i -XPOST 'http://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/indexing/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/test/indexing?split=true&koreng=true&chosung=false' -H 'Content-Type:application/json; charset=UTF-8' -d '
            [
              {
                "input": "ナイキ",
                "weight": 3
              },
              {
                "input": "ナイキ 運動靴",
                "weight": 2
              },
              {
                "input": "運動靴",
                "weight": 1
              }
            ]'
            ```

    - Response

        ```
        {
          "id" : 1
        }
        ```

  - インデックス結果確認API

    - Request

        ```
        curl -i -XGET 'https://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/indexing/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/test/indexing_log?id=1'
        ```

        - id 1は、上記インデックス API Responseのidです。

    - Response

        ```
        {
          "request_time" : "2017-10-23T12:36:43",
          "file_name" : "documents.json",
          "file_size" : 114,
          "status" : 4
        }
        ```

        - status
            - 1 :待機中
            - 2 : 無視された
            - 3 :進行中
            - 4 :成功
            - 5 :失敗

### 3. オートコンプリート

**オートコンプリート方法**

1. **オートコンプリート**タブをクリックします。

2. 検索する単語を入力します。

3. 出力数を指定します。

4. オートコンプリートREST APIです。

5. オートコンプリート結果が出力されます。

![img](https://camo.githubusercontent.com/7b5ba9c39e5e35c5a4f6873f2cdb06bdd0bc1eb7/687474703a2f2f7374617469632e746f6173746f76656e2e6e65742f70726f645f6175746f636f6d706c6574652f6175746f636f6d706c6574655f70726f6365647572652d32303230303131372e313532312e706e67)

**REST API**

  - Request

    ```
    curl -G -XGET 'http://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/autocomplete/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/test/autocomplete?count=10' --data-urlencode query='ナ'
    ```

  - Response

    ```
    {
      "collections" : [ {
        "index" : 0,
        "items" : [ [ "ナイキ" ], [ "ナイキ 運動靴" ] ],
        "title" : ""
      } ],
      "query" : [ "ナ", "sk" ],
      "ver" : "1.0"
    }
    ```

### 4. ACL

インデックス およびオートコンプリートREST APIを呼び出すことができる端末のIPを制限できます。
コンソールでテストする場合、ACL設定と関係ありません。

**ACLの設定方法**

1. **ACL**タブをクリックします。

2. インデックスをリクエストするIPアドレスが202.179.177.21の場合のみインデックスができるように設定した例です。

3. オートコンプリートリクエストは、すべてのIPからできるように設定した例です。

4. **保存**ボタンをクリックします。

![img](https://camo.githubusercontent.com/299f6d5330cad8525920c9a32da94d5ba55716ac/687474703a2f2f7374617469632e746f6173746f76656e2e6e65742f70726f645f6175746f636f6d706c6574652f61636c5f70726f6365647572652d32303230303131372e313532352e706e67)



## 機能詳細説明

### 中間マッチング

**インデックス**

テストを行うためにデータをインデックスします。

```
[
  {
    "input": "ナイキ運動靴",
    "weight": 2
  },
  {
    "input": "アディダス 運動靴",
    "weight": 1
  }
]
```

インデックスする時、**中間マッチング**を選択します。

[![img](https://camo.githubusercontent.com/3c8342ec8fe686cf686281c1cccbedf5dcbb6660/687474703a2f2f7374617469632e746f6173746f76656e2e6e65742f70726f645f6175746f636f6d706c6574652f696e6669782d696e646578696e672d32303230303131372e313631362e706e67)](https://camo.githubusercontent.com/3c8342ec8fe686cf686281c1cccbedf5dcbb6660/687474703a2f2f7374617469632e746f6173746f76656e2e6e65742f70726f645f6175746f636f6d706c6574652f696e6669782d696e646578696e672d32303230303131372e313631362e706e67)

**オートコンプリート**

1. **運動**を入力します。

2. 中間に**運動**がある**ナイキ運動靴**が出力されます。

![img](https://camo.githubusercontent.com/9c8d7a33edc2af8720362ea09194d2fb8fffbe74/687474703a2f2f7374617469632e746f6173746f76656e2e6e65742f70726f645f6175746f636f6d706c6574652f696e6669782d737567676573742d32303230303131372e313631392e706e67)


### 韓国語/英語入力変換

**インデックス**

テストを行うためにデータをインデックスします。

```
[
  {
    "input": "ナイキ",
    "weight": 2
  },
  {
    "input": "アディダス",
    "weight": 1
  }
]
```

インデックスする時、**韓国語/英語入力変換**を選択します。

[![img](https://camo.githubusercontent.com/b8c6fa079d8560e637d937227fad5a9f24472709/687474703a2f2f7374617469632e746f6173746f76656e2e6e65742f70726f645f6175746f636f6d706c6574652f6b6f72656e672d696e646578696e672d32303230303131372e313632332e706e67)](https://camo.githubusercontent.com/b8c6fa079d8560e637d937227fad5a9f24472709/687474703a2f2f7374617469632e746f6173746f76656e2e6e65742f70726f645f6175746f636f6d706c6574652f6b6f72656e672d696e646578696e672d32303230303131372e313632332e706e67)

**オートコンプリート**

1. 「나이」の英語入力である「skdl」を入力します。

2. 「나이키」が出力されます。

![img](https://camo.githubusercontent.com/6ed22f7ead71302064ae61184087b64745cdca8a/687474703a2f2f7374617469632e746f6173746f76656e2e6e65742f70726f645f6175746f636f6d706c6574652f6b6f72656e672d737567676573742d32303230303131372e313633302e706e67)


### 初声オートコンプリート

**インデックス**

テストを行うためにデータをインデックスします。

```
[
  {
    "input": "ナイキ",
    "weight": 2
  },
  {
    "input": "アディダス",
    "weight": 1
  }
]
```

  - インデックスする時、**初声オートコンプリート**をチェックします。

[![img](https://camo.githubusercontent.com/083dc9ba9a8dc5f6afabe074c69dde39e150c9f7/687474703a2f2f7374617469632e746f6173746f76656e2e6e65742f70726f645f6175746f636f6d706c6574652f63686f73756e672d696e646578696e672d32303230303131372e313633322e706e67)](https://camo.githubusercontent.com/083dc9ba9a8dc5f6afabe074c69dde39e150c9f7/687474703a2f2f7374617469632e746f6173746f76656e2e6e65742f70726f645f6175746f636f6d706c6574652f63686f73756e672d696e646578696e672d32303230303131372e313633322e706e67)

**オートコンプリート**

1. 「ㄴㅇㅋ」を入力します。

2. 「나이키」が出力されます。

![img](https://camo.githubusercontent.com/4eb769261d3f0687958baff8dfb5ba27695903f4/687474703a2f2f7374617469632e746f6173746f76656e2e6e65742f70726f645f6175746f636f6d706c6574652f63686f73756e672d737567676573742d32303230303131372e313735332e706e67)


### 付加情報出力

**インデックス**

テストを行うためにデータをインデックスします。

```
[
  {
    "input": "ナイキ",
    "payload": ["http://image.nhnent.com/images/nike.jpg", "ブランド > スポーツ"],
    "weight": 2
  },
  {
    "input": "アディダス",
    "payload": ["http://image.nhnent.com/images/adidas.jpg", "ブランド > スポーツ"],
    "weight": 1
  }
]
```

ペイロード(payload)に出力したい付加情報を入力します。

**オートコンプリート**

1. 入力した付加情報(イメージURL、カテゴリー)が出力されます。

![img](https://camo.githubusercontent.com/6b3a3900fd78396e735ea78a15572a7526dcd85b/687474703a2f2f7374617469632e746f6173746f76656e2e6e65742f70726f645f6175746f636f6d706c6574652f737567676573742d7061796c6f61642d32303230303131372e313533322e706e67)



### Input/Outputを別々に設定

**インデックス**

テストを行うためにデータをインデックスします。

```
[
  {
    "input": "ナイキ",
    "output": "Nike",
    "weight": 2
  },
  {
    "input": "アディダス",
    "output": "Adidas",
    "weight": 1
  }
]
```

**オートコンプリート**

1. 「나」を入力入力します。

2. 「Nike」が出力されます。

![img](https://camo.githubusercontent.com/58cc333c46417956e3881bc908791368c7dfcc67/687474703a2f2f7374617469632e746f6173746f76656e2e6e65742f70726f645f6175746f636f6d706c6574652f737567676573742d6f75747075742d32303230303131372e313533362e706e67)


### マルチサービス

2個以上のサービスのオートコンプリート結果を1回のオートコンプリートAPIリクエストで出力する機能です。例えば、ブランドとカテゴリーのオートコンプリートを1回のAPIリクエストで出力する時に使用します。

**ブランドサービスの作成**

**サービス作成**ボタンをクリックした後、**サービス作成**ウィンドウでIDを入力し、**作成**ボタンをクリックします。

[![img](https://camo.githubusercontent.com/a1ce9dc03fe963ec1c8038e60eac25650830d4e8/687474703a2f2f7374617469632e746f6173746f76656e2e6e65742f70726f645f6175746f636f6d706c6574652f646f6d61696e5f6372656174652d6272616e642d32303230303131372e313634362e706e67)](https://camo.githubusercontent.com/a1ce9dc03fe963ec1c8038e60eac25650830d4e8/687474703a2f2f7374617469632e746f6173746f76656e2e6e65742f70726f645f6175746f636f6d706c6574652f646f6d61696e5f6372656174652d6272616e642d32303230303131372e313634362e706e67)

**ブランドインデックス**

テストを行うためにデータをインデックスします。

```
[
  {
    "input": "ナイキ",
    "weight": 2
  },
  {
    "input": "アディダス",
    "weight": 1
  }
]
```

**カテゴリーサービスの作成**

**サービス作成**ボタンをクリックした後、**サービス作成**ウィンドウでIDを入力し、**作成**ボタンをクリックします。

[![img](https://camo.githubusercontent.com/efb7010501ba3b26a887ef566e2f882dd95b4b36/687474703a2f2f7374617469632e746f6173746f76656e2e6e65742f70726f645f6175746f636f6d706c6574652f646f6d61696e5f6372656174652d63617465676f72792d32303230303131372e313635312e706e67)](https://camo.githubusercontent.com/efb7010501ba3b26a887ef566e2f882dd95b4b36/687474703a2f2f7374617469632e746f6173746f76656e2e6e65742f70726f645f6175746f636f6d706c6574652f646f6d61696e5f6372656174652d63617465676f72792d32303230303131372e313635312e706e67)

**カテゴリーインデックス**

テストを行うためにデータをインデックスします。

```
[
  {
    "input": "メンズバッグ",
    "weight": 2
  },
  {
    "input": "アウター",
    "weight": 1
  }
]
```

**オートコンプリート**

API呼び出し時、「serviceids/brand,category」にリクエストしました。

APIレスポンスにindex 0はbrand、index 1はcategoryオートコンプリート結果が出力されます。

```
curl -G -XGET 'http://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/autocomplete/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/brand,category/autocomplete?count=10' --data-urlencode query='ナ'
{
  "collections" : [ {
    "index" : 0,
    "items" : [ [ "ナイキ" ] ],
    "title" : ""
  }, {
    "index" : 1,
    "items" : [ [ "メンズバッグ" ] ],
    "title" : ""
  } ],
  "query" : [ "나", "sk" ],
  "ver" : "1.0"
}
```



## 詳細ガイド

### 出力優先順位

インデックスファイルが下記のような場合、ユーザーが「ㄴ」を入力すると「ノートパソコン」、「ナイキ」、「メンズトップス」の順に出力されます。

```
[
  {
    "input": "ノートパソコン",
    "weight": 3
  },
  {
    "input": "ナイキ",
    "weight": 2
  },
  {
    "input": "メンズトップス",
    "weight": 1
  }
]
```

  - 「ㄴ」で始まる単語のうち、weightが高い順に出力されます。

- conversion_weights

 原本、中間マッチング、韓国語/英語入力変換、 初声オートコンプリートの出力順序を調節できます。

  - 例

    ```
    curl -i -XPOST 'http://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/indexing/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/test/indexing?split=true&koreng=true&chosung=true&conversion_weights=17,16,15,14,13,12,11,10' -H 'Content-Type:application/json; charset=UTF-8' -d '
    [
      {
        "input": "ナイキ",
        "weight": 3
      },
      {
        "input": "運動靴",
        "weight": 2
      },
      {
        "input": "ナイキ エアマックス",
        "weight": 1
      }
    ]'
    ```

    - conversion_weightsに "17,16,15,14,13,12,11,10"を指定しました。

  - conversion_weightsの各Indexの意味

    | Index | Description                    |
    | ----- | ------------------------------ |
    | 0     | 原本                          |
    | 1     | 原本の韓国語/英語入力変換            |
    | 2     | 原本の初声                    |
    | 3     | 原本の初声の韓国語/英語入力変換     |
    | 4     | 中間マッチング                      |
    | 5     | 中間マッチングの韓国語/英語入力変換       |
    | 6     | 中間マッチングの初声               |
    | 7     | 中間マッチングの初声の韓国語/英語入力変換 |

  - サンプルデータのインデックス結果

    | Key              | Relevance                              | Description                                        |
    | ---------------- | -------------------------------------- | -------------------------------------------------- |
    | ナイキ           | 20 = 3(weight) + 17(conversion weight) | "ナイキ"の原本                                   |
    | 運動靴           | 19 = 2(weight) + 17(conversion weight) | "運動靴"の原本                                   |
    | ナイキ エアマックス  | 18 = 1(weight) + 17(conversion weight) | "ナイキ エアマックス"の原本                          |
    | skdlzl           | 19 = 3(weight) + 16(conversion weight) | "ナイキ"の韓国語/英語入力変換                            |
    | dnsehdghk        | 18 = 2(weight) + 16(conversion weight) | "運動靴"の韓国語/英語入力変換                            |
    | skdlzl dpdjaortm | 17 = 1(weight) + 16(conversion weight) | "ナイキ エアマックス"の韓国語/英語入力変換                   |
    | ㄴㅇㅋ          | 18 = 3(weight) + 15(conversion weight) | "ナイキ"の初声                                    |
    | ㅇㄷㅎ          | 17 = 2(weight) + 15(conversion weight) | "運動靴"の初声                                    |
    | ㄴㅇㅋㅇㅇㅁㅅ | 16 = 1(weight) + 15(conversion weight) | "'ナイキ エアマックス"の初声                          |
    | sdz              | 17 = 3(weight) + 14(conversion weight) | "ナイキ"の初声の韓国語/英語入力変換                     |
    | deg              | 16 = 2(weight) + 14(conversion weight) | "運動靴"の初声の韓国語/英語入力変換                     |
    | sdz ddat         | 15 = 1(weight) + 14(conversion weight) | "ナイキ エアマックス"の初声の韓国語/英語入力変換            |
    | エアマックス         | 14 = 1(weight) + 13(conversion weight) | "ナイキ エアマックス"の中間マッチング                      |
    | dpdjaortm        | 13 = 1(weight) + 12(conversion weight) | "ナイキ エアマックス"の中間マッチングの韓国語/英語入力変換       |
    | ㅇㅇㅁㅅ        | 12 = 1(weight) + 11(conversion weight) | "ナイキ エアマックス"の中間マッチングの初声               |
    | ddat             | 11 = 1(weight) + 10(conversion weight) | "ナイキ エアマックス"の中間マッチングの初声の韓国語/英語入力変換 |

    - ユーザーが「ㅇ」を入力した時、「運動靴」(relevance 19)が「エアマックス」(relevance 14)より先に出力されます。

### ACL

ACLの設定画面は次のとおりです。

[![img](https://camo.githubusercontent.com/7e9a17d67a5611786da108de51a5baef6a3999ab/687474703a2f2f7374617469632e746f6173746f76656e2e6e65742f70726f645f6175746f636f6d706c6574652f61636c2d64657461696c2d32303230303131372e313635342e706e67)](https://camo.githubusercontent.com/7e9a17d67a5611786da108de51a5baef6a3999ab/687474703a2f2f7374617469632e746f6173746f76656e2e6e65742f70726f645f6175746f636f6d706c6574652f61636c2d64657461696c2d32303230303131372e313635342e706e67)

- 入力形式
  - IP形式で入力できます。
    - 例) 202.179.177.21
  - CIDR形式で入力できます。
    - 例) 202.179.177.0/24
  - IPまたはCIDRを複数入力できます。
    - 例) 202.179.177.21, 202.179.177.0/24
  - allの場合、すべてマッチングされます。
    - 値が空の場合、すべてマッチングされません。  
- 許可、拒否のどちらにもマッチングされる場合、拒否されます。
- 許可、拒否のどちらにもマッチングされない場合、拒否されます。

## クライアントサンプルコード

次はファイルアップロード方式のインデックスサンプルコードです。

### java

- dependency

```
compile group: 'org.apache.httpcomponents', name: 'httpclient', version: '4.5.6'
compile group: 'org.apache.httpcomponents', name: 'httpmime', version: '4.5.6'
```

- インデックス(ファイルアップロード方式)

```
package com.toast.cloud.autocomplete.client;

import org.apache.http.HttpEntity;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.ResponseHandler;
import org.apache.http.client.methods.HttpUriRequest;
import org.apache.http.client.methods.RequestBuilder;
import org.apache.http.entity.ContentType;
import org.apache.http.entity.mime.HttpMultipartMode;
import org.apache.http.entity.mime.MultipartEntityBuilder;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;

import java.io.File;
import java.io.IOException;
import java.io.PrintWriter;

public class IndexingClient {

	public static void main(String[] args) throws IOException {

		String documents = ""
			+ "[\n"
			+ "  {\n"
			+ "	   \"input\": \"ナイキ\",\n"
			+ "    \"weight\": 3\n"
			+ "  }\n"
			+ "]\n";

		File tempFile = File.createTempFile("documents-",".json", new File("/tmp/"));
		tempFile.deleteOnExit();

		PrintWriter printWriter = new PrintWriter(tempFile);
		printWriter.println(documents);
		printWriter.close();

		try (CloseableHttpClient httpclient = HttpClients.createDefault()) {

			// build multipart upload request.
			HttpEntity data = MultipartEntityBuilder.create()
				.setMode(HttpMultipartMode.BROWSER_COMPATIBLE)
				.addBinaryBody("file", tempFile, ContentType.DEFAULT_BINARY, tempFile.getName())
				.build();

			// build http request and assign multipart upload data.
			HttpUriRequest request = RequestBuilder
				.post("https://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/indexing/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/test/indexing?split=true&koreng=true&chosung=false")
				.setEntity(data)
				.build();

			System.out.println("Executing request " + request.getRequestLine());

			// Create a custom response handler.
			ResponseHandler<String> responseHandler = response -> {
				int status = response.getStatusLine().getStatusCode();
				if (status >= 200 && status < 300) {
					HttpEntity entity = response.getEntity();
					return entity != null ? EntityUtils.toString(entity) : null;
				} else {
					throw new ClientProtocolException("Unexpected response status: " + status);
				}
			};
			String responseBody = httpclient.execute(request, responseHandler);
			System.out.println(responseBody);
		}
	}
}
```

### php

- インデックス(ファイルアップロード方式)

```
<?php
    $documents = ""
      . "[\n"
      . "  {\n"
      . "    \"input\": \"ナイキ\",\n"
      . "    \"weight\": 3\n"
      . "  }\n"
      . "]\n";

    $file = DIRECTORY_SEPARATOR.trim(sys_get_temp_dir(), DIRECTORY_SEPARATOR).DIRECTORY_SEPARATOR.ltrim("documents.json", DIRECTORY_SEPARATOR);
    file_put_contents($file, $documents);

    register_shutdown_function(function() use($file) {
        unlink($file);
    });

    $data = array(
        'file' => curl_file_create($file, "application/json", basename($file))
    );

    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL,"https://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/indexing/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/test/indexing?split=true&koreng=true&chosung=false");
    curl_setopt($ch, CURLOPT_POST, 1);
    curl_setopt($ch, CURLOPT_HTTPHEADER, array("Content-Type:multipart/form-data; charset=UTF-8"));
    curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_HEADER, true);

    $response = curl_exec($ch) or die(curl_error($ch));
    print_r($response);

    curl_close($ch);
?>
```
