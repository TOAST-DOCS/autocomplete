## Search > Autocomplete > コンソール使用ガイド

## 注意

- 文書内のホスト名「api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com」は、ユーザーごとに異なる場合があります。
- 文書内のアプリケーションキー「7IkFjTvxA8zwfL8e」は、ユーザーごとに異なります。

## 始める

まずAutocompleteサービスを有効化します。

1. **TOAST Console**で**サービス選択**をクリックします。

2. **Autocomplete**をクリックします。

![img](http://static.toastoven.net/prod_autocomplete/product-use-02-ja-20200304.jpg)

サービスが有効になっているかを確認する方法は次のとおりです。

1. **TOAST Console**左側のメニューで**Search**をクリックします。

2. **Autocomplete**が表示されていれば、サービスが有効になっているということです。

![img](http://static.toastoven.net/prod_autocomplete/product-use-03-ja-20200304.jpg)

## 基本使用方法

### 1. サービスの作成
1. **サービス作成**ボタンをクリックします。

2. **サービス作成**ウィンドウでサービスIDを入力します。

    - 英字小文字、数字およびアーダースコア(_)とハイフン(-)のみ使用できます。
    - 最初の文字に数字、アーダースコア(_)、ハイフン(-)は使用できません。
    - 2文字以上入力する必要があります。

3. **保存**ボタンをクリックします。

![img](http://static.toastoven.net/prod_autocomplete/domain_create_procedure-ja-20200304.jpg)

作成されたサービス結果を確認します。  
1. 作成されたサービスID(test)をクリックします。

![img](http://static.toastoven.net/prod_autocomplete/domain_create_result-ja-20200304.jpg)

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

![img](http://static.toastoven.net/prod_autocomplete/indexing_procedure-01-ja-20200304.jpg)  
![img](http://static.toastoven.net/prod_autocomplete/indexing_procedure-02-ja-20200304.jpg)

**インデックスの注意事項**

インデックスをリクエストすると、既存のデータはすべて削除され、新規データに変更されます。



**REST API**

  - インデックスAPI

    - Request

        - ファイルアップロード方式

            ```
            curl -XPOST 'http://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/indexing/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/test/indexing?split=true&koreng=true&chosung=false' -H 'Accept-Language:ja' -H 'Content-Type:multipart/form-data; charset=UTF-8' -F 'file=@documents.json'
            ```

        - Payload方式

            ```
            curl -i -XPOST 'http://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/indexing/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/test/indexing?split=true&koreng=true&chosung=false' -H 'Accept-Language:ja' -H 'Content-Type:application/json; charset=UTF-8' -d '
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
        curl -i -XGET 'https://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/indexing/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/test/indexing_log?id=1' -H 'Accept-Language:ja'
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

![img](http://static.toastoven.net/prod_autocomplete/autocomplete_procedure-ja-20200304.jpg)

**REST API**

  - Request

    ```
    curl -G -XGET 'http://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/autocomplete/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/test/autocomplete?count=10' -H 'Accept-Language:ja' --data-urlencode query='ナ'
    ```

  - Response

    ```
    {
      "collections" : [ {
        "index" : 0,
        "items" : [ [ "ナイキ" ], [ "ナイキ 運動靴" ] ],
        "title" : ""
      } ],
      "query" : [ "ナ"],
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

![img](http://static.toastoven.net/prod_autocomplete/acl_procedure-ja-20200304.jpg)



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

![img](http://static.toastoven.net/prod_autocomplete/infix-indexing-ja-20200304.jpg)

**オートコンプリート**

1. **運動**を入力します。

2. 中間に**運動**がある**ナイキ運動靴**が出力されます。

![img](http://static.toastoven.net/prod_autocomplete/infix-suggest-ja-20200304.jpg)

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

![img](http://static.toastoven.net/prod_autocomplete/suggest-payload-ja-20200304.jpg)



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

1. 「ナ」を入力します。

2. 「Nike」が出力されます。

![img](http://static.toastoven.net/prod_autocomplete/suggest-output-ja-20200304.jpg)

### マルチサービス

2個以上のサービスのオートコンプリート結果を1回のオートコンプリートAPIリクエストで出力する機能です。例えば、ブランドとカテゴリーのオートコンプリートを1回のAPIリクエストで出力する時に使用します。

**ブランドサービスの作成**

**サービス作成**ボタンをクリックした後、**サービス作成**ウィンドウでIDを入力し、**作成**ボタンをクリックします。

![img](http://static.toastoven.net/prod_autocomplete/domain_create-brand-ja-20200304.jpg)

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

![img](http://static.toastoven.net/prod_autocomplete/domain_create-category-ja-20200304.jpg)

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

- Request

    API呼び出し時、「serviceids/brand,category」にリクエストしました。

    ```
    curl -G -XGET 'http://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/autocomplete/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/brand,category/autocomplete?count=10' -H 'Accept-Language:ja' --data-urlencode query='ア'
    ```
- Response

    APIレスポンスにindex 0はbrand、index 1はcategoryオートコンプリート結果が出力されます。

    ```
    curl -G -XGET 'http://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/autocomplete/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/brand,category/autocomplete?count=10' -H 'Accept-Language:ja' --data-urlencode query='ア'
    {
      "collections" : [ {
        "index" : 0,
        "items" : [ [ "アディダス" ] ],
        "title" : ""
      }, {
        "index" : 1,
        "items" : [ [ "アウター" ] ],
        "title" : ""
      } ],
      "query" : [ "ア"],
      "ver" : "1.0"
    }
    ```

## 詳細ガイド

### 出力優先順位

インデックスファイルが下記のような場合、ユーザーが「ナ」を入力すると「スニーカー」、「スイッチ」、「スマホケース」の順に出力されます。

```
[
  {
    "input": "スニーカー",
    "weight": 3
  },
  {
    "input": "スイッチ",
    "weight": 2
  },
  {
    "input": "スマホケース",
    "weight": 1
  }
]
```
- 「ナ」で始まる単語のうち、weightが高い順に出力されます。

**conversion_weights**

原本、中間マッチングの出力順序を調節できます。

- 例

    ```
    curl -i -XPOST 'http://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/indexing/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/test/indexing?split=true&koreng=true&chosung=true&conversion_weights=17,0,0,0,13,0,0,0' -H 'Accept-Language:ja' -H 'Content-Type:application/json; charset=UTF-8' -d '
    [
      {
        "input": "ナイキ スニーカー",
        "weight": 2
      },
      {
        "input": "スマホケース",
        "weight": 1
      }
    ]'
    ```

    - conversion_weightsに "17,0,0,0,13,0,0,0"を指定しました。

- conversion_weightsの各Indexの意味

    | Index | Description                    |
    | ----- | ------------------------------ |
    | 0     | 原本                            |
    | 1     | Not available for japanese     |
    | 2     | Not available for japanese     |
    | 3     | Not available for japanese     |
    | 4     | 中間マッチング                     |
    | 5     | Not available for japanese     |
    | 6     | Not available for japanese     |
    | 7     | Not available for japanese     |

- サンプルデータのインデックス結果

    | Key              | Relevance                              | Description                                        |
    | ---------------- | -------------------------------------- | -------------------------------------------------- |
    | ナイキ スニーカー     | 19 = 2(weight) + 17(conversion weight) | "ナイキ スニーカー"の原本                                   |
    | スニーカー          | 15 = 2(weight) + 13(conversion weight) | "ナイキ スニーカー"の中間マッチング                      |
    | スマホケース         | 18 = 1(weight) + 17(conversion weight) | "スマホケース"の原本                          |

    - ユーザーが「ナ」を入力した時、「スマホケース」(relevance 18)が「スニーカー」(relevance 15)より先に出力されます。

### ACL

ACLの設定画面は次のとおりです。

![img](http://static.toastoven.net/prod_autocomplete/acl-detail-ja-20200304.jpg)

**入力形式**

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
