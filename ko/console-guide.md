## Search > Autocomplete > 콘솔 사용 가이드

## 알아두기

- 문서 내의 호스트명 'api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com'는 사용자별로 다를 수 있습니다.
- 문서 내의 앱키 '7IkFjTvxA8zwfL8e'는 사용자별로 다릅니다.

## 시작하기

먼저 Autocomplete 서비스를 활성화합니다.

1. **TOAST Console**에서 **서비스 선택**을 클릭합니다.

2. **Autocomplete**을 클릭합니다.

![img](http://static.toastoven.net/prod_autocomplete/product-use-02-ko-20200304.jpg)

서비스가 활성화되었는지 확인하는 방법은 다음과 같습니다.

1. **TOAST Console** 왼쪽 메뉴에서 **Search**를 클릭합니다.

2. **Autocomplete**가 보이면 서비스가 활성화된 것입니다.

![img](http://static.toastoven.net/prod_autocomplete/product-use-03-ko-20200304.jpg)

## 기본 사용법

### 1. 서비스 생성
1. **서비스 생성** 버튼을 클릭합니다.

2. **서비스 생성** 창에서 서비스 ID를 입력합니다.

    - 영어 소문자, 숫자 및 \_(밑줄)과 -(하이픈)만 사용할 수 있습니다.
    - 숫자, \_(밑줄), -(하이픈)로 시작할 수 없습니다.
    - 최소 두 글자 이상 가능합니다.

3. **저장** 버튼을 클릭합니다.

![img](http://static.toastoven.net/prod_autocomplete/domain_create_procedure-ko-20200304.jpg)

생성된 서비스 결과를 확인합니다.  
1. 생성된 서비스 ID(test)를 클릭합니다.

![img](http://static.toastoven.net/prod_autocomplete/domain_create_result-ko-20200304.jpg)

### 2. 색인

색인할 파일을 생성하고 색인하는 방법은 다음과 같습니다.

**색인 파일 생성**

  - 아래 예제와 같은 형식으로 색인 요청 파일을 생성합니다.
  - 색인할 파일은 UTF-8로 생성해야 합니다.
      - Windows 메모장에서 파일 저장 시 인코딩을 UTF-8로 지정해서 저장합니다.
  - 예제에서는 data/documents.json 이름으로 생성했습니다.
  - 최대 파일 크기는 32MB입니다.

```
[
  {
    "input": "나이키",
    "weight": 3
  },
  {
    "input": "나이키 운동화",
    "weight": 2
  },
  {
    "input": "운동화",
    "weight": 1
  }
]
```

**색인 방법**

1. **색인** 탭을 클릭합니다.

2. **파일 선택** 버튼을 클릭합니다.

3. 색인할 파일을 선택합니다.

4. **열기** 버튼을 클릭합니다.

5. 색인 명령어가 REST API로 출력됩니다.

6. **색인** 버튼을 클릭합니다.

7. **새로 고침** 버튼을 클릭합니다.

8. 색인 결과를 확인합니다.

![img](http://static.toastoven.net/prod_autocomplete/indexing_procedure-01-ko-20200304.jpg)  
![img](http://static.toastoven.net/prod_autocomplete/indexing_procedure-02-ko-20200304.jpg)

**색인 주의 사항**

색인을 요청하면 기존 데이터는 모두 삭제되고 신규 데이터로 교체됩니다.



**REST API**

  - 색인 API

    - Request

        - 파일 업로드 방식

            ```
            curl -XPOST 'http://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/indexing/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/test/indexing?split=true&koreng=true&chosung=false' -H 'Content-Type:multipart/form-data; charset=UTF-8' -F 'file=@documents.json'
            ```

        - Payload 방식

            ```
            curl -i -XPOST 'http://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/indexing/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/test/indexing?split=true&koreng=true&chosung=false' -H 'Content-Type:application/json; charset=UTF-8' -d '
            [
              {
                "input": "나이키",
                "weight": 3
              },
              {
                "input": "나이키 운동화",
                "weight": 2
              },
              {
                "input": "운동화",
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

  - 색인 결과 확인 API

    - Request

        ```
        curl -i -XGET 'https://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/indexing/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/test/indexing_log?id=1'
        ```

        - id 1은 위의 색인 API Response 의 id입니다.

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
            - 1 : 대기 중
            - 2 : 무시됨
            - 3 : 진행 중
            - 4 : 성공
            - 5 : 실패

### 3. 자동 완성

**자동 완성 방법**

1. **자동 완성** 탭을 클릭합니다.

2. 검색할 단어를 입력합니다.

3. 출력 개수를 지정합니다.

4. 자동 완성 REST API입니다.

5. 자동 완성 결과가 출력됩니다.

![img](http://static.toastoven.net/prod_autocomplete/autocomplete_procedure-ko-20200304.jpg)

**REST API**

  - Request

    ```
    curl -G -XGET 'http://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/autocomplete/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/test/autocomplete?count=10' --data-urlencode query='나'
    ```

  - Response

    ```
    {
      "collections" : [ {
        "index" : 0,
        "items" : [ [ "나이키" ], [ "나이키 운동화" ] ],
        "title" : ""
      } ],
      "query" : [ "나", "sk" ],
      "ver" : "1.0"
    }
    ```

### 4. ACL

색인 및 자동완성 REST API를 호출할 수 있는 장비의 IP를 제한할 수 있습니다.
콘솔에서 테스트하는 경우 ACL 설정과 관련 없습니다.

**ACL 설정 방법**

1. **ACL** 탭을 클릭합니다.

2. 색인 요청 IP 주소가 202.179.177.21 인 경우만 색인이 가능하도록 설정한 예제입니다.

3. 자동완성 요청은 모든 IP에서 가능하도록 설정한 예제입니다.

4. **저장** 버튼을 클릭합니다.

![img](http://static.toastoven.net/prod_autocomplete/acl_procedure-ko-20200304.jpg)



## 기능 상세 설명

### 중간 매칭

**색인**

테스트를 위해 아래 데이터를 색인합니다.

```
[
  {
    "input": "나이키운동화",
    "weight": 2
  },
  {
    "input": "아디다스 운동화",
    "weight": 1
  }
]
```

색인할 때 **중간 매칭**을 선택합니다.

![img](http://static.toastoven.net/prod_autocomplete/infix-indexing-ko-20200304.jpg)

**자동 완성**

1. **운동**을 입력합니다.

2. 중간에 **운동**으로 시작되는 **나이키운동화**가 출력됩니다.

![img](http://static.toastoven.net/prod_autocomplete/infix-suggest-ko-20200304.jpg)


### 한영타 변환

**색인**

테스트를 위해 아래 데이터를 색인합니다.

```
[
  {
    "input": "나이키",
    "weight": 2
  },
  {
    "input": "아디다스",
    "weight": 1
  }
]
```

색인할 때 **한영타 변환**을 선택합니다.

![img](http://static.toastoven.net/prod_autocomplete/koreng-indexing-ko-20200304.jpg)

**자동 완성**

1. '나이'의 영타인 'skdl'를 입력합니다.

2. '나이키'가 출력됩니다.

![img](http://static.toastoven.net/prod_autocomplete/koreng-suggest-ko-20200304.jpg)


### 초성 자동 완성

**색인**

테스트를 위해 아래 데이터를 색인합니다.

```
[
  {
    "input": "나이키",
    "weight": 2
  },
  {
    "input": "아디다스",
    "weight": 1
  }
]
```

  - 색인할 때 **초성 자동완성**을 체크합니다.

![img](http://static.toastoven.net/prod_autocomplete/chosung-indexing-ko-20200304.jpg)

**자동 완성**

1. 'ㄴㅇㅋ'를 입력합니다.

2. '나이키'가 출력됩니다.

![img](http://static.toastoven.net/prod_autocomplete/chosung-suggest-ko-20200304.jpg)


### 부가 정보 출력

**색인**

테스트를 위해 아래 데이터를 색인합니다.

```
[
  {
    "input": "나이키",
    "payload": ["http://image.nhnent.com/images/nike.jpg", "브랜드>스포츠"],
    "weight": 2
  },
  {
    "input": "아디다스",
    "payload": ["http://image.nhnent.com/images/adidas.jpg", "브랜드>스포츠"],
    "weight": 1
  }
]
```

페이로드(payload)에 출력하고 싶은 부가 정보를 입력합니다.

**자동 완성**

1. 입력한 부가 정보(이미지 URL, 카테고리)가 출력됩니다.

![img](http://static.toastoven.net/prod_autocomplete/suggest-payload-ko-20200304.jpg)



### Input/Output을 다르게 설정

**색인**

테스트를 위해 아래 데이터를 색인합니다.

```
[
  {
    "input": "나이키",
    "output": "Nike",
    "weight": 2
  },
  {
    "input": "아디다스",
    "output": "Adidas",
    "weight": 1
  }
]
```

**자동 완성**

1. '나'를 입력입력합니다.

2. 'Nike'가 출력됩니다.

![img](http://static.toastoven.net/prod_autocomplete/suggest-output-ko-20200304.jpg)


### 멀티 서비스

2개 이상 서비스의 자동 완성 결과를 한 번의 자동 완성 API 요청으로 출력하는 기능입니다. 예를 들어 브랜드와 카테고리 자동 완성을 한 번의 API 요청으로 출력할 때 사용합니다.

**브랜드 서비스 생성**

**서비스 생성** 버튼을 클릭한 후 **서비스 생성** 창에서 ID를 입력하고 **생성** 버튼을 클릭합니다.

![img](http://static.toastoven.net/prod_autocomplete/domain_create-brand-ko-20200304.jpg)

**브랜드 색인**

테스트를 위해 아래 데이터를 색인합니다.

```
[
  {
    "input": "나이키",
    "weight": 2
  },
  {
    "input": "아디다스",
    "weight": 1
  }
]
```

**카테고리 서비스 생성**

**서비스 생성** 버튼을 클릭한 후 **서비스 생성** 창에서 ID를 입력하고 **생성** 버튼을 클릭합니다.

![img](http://static.toastoven.net/prod_autocomplete/domain_create-category-ko-20200304.jpg)

**카테고리 색인**

테스트를 위해 아래 데이터를 색인합니다.

```
[
  {
    "input": "남성가방",
    "weight": 2
  },
  {
    "input": "아우터",
    "weight": 1
  }
]
```

**자동 완성**

API 호출 시 'serviceids/brand,category'로 요청했습니다.

API 응답에 index 0은 brand, index 1은 category 자동 완성 결과가 출력됩니다.

```
curl -G -XGET 'http://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/autocomplete/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/brand,category/autocomplete?count=10' --data-urlencode query='나'
{
  "collections" : [ {
    "index" : 0,
    "items" : [ [ "나이키" ] ],
    "title" : ""
  }, {
    "index" : 1,
    "items" : [ [ "남성가방" ] ],
    "title" : ""
  } ],
  "query" : [ "나", "sk" ],
  "ver" : "1.0"
}
```



## 상세 가이드

### 출력 우선 순위

**설정 방법**

색인 파일이 아래와 같을 경우 사용자가 'ㄴ' 을 입력하면 '노트북', '나이키', '남성상의'순으로 출력됩니다.

```
[
  {
    "input": "노트북",
    "weight": 3
  },
  {
    "input": "나이키",
    "weight": 2
  },
  {
    "input": "남성상의",
    "weight": 1
  }
]
```  
- 'ㄴ'으로 시작하는 단어 중 weight가 높은 순서대로 출력됩니다.

**conversion_weights**

원본, 중간 매칭, 한영타 변환, 초성 자동 완성의 출력 순서를 조절할 수 있습니다.

- 예제

    ```
    curl -i -XPOST 'http://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/indexing/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/test/indexing?split=true&koreng=true&chosung=true&conversion_weights=17,16,15,14,13,12,11,10' -H 'Content-Type:application/json; charset=UTF-8' -d '
    [
      {
        "input": "나이키",
        "weight": 3
      },
      {
        "input": "운동화",
        "weight": 2
      },
      {
        "input": "나이키 에어맥스",
        "weight": 1
      }
    ]'
    ```

    - conversion_weights로 "17,16,15,14,13,12,11,10"을 지정했습니다.

- conversion_weights의 Index별 의미

    | Index | Description                    |
    | ----- | ------------------------------ |
    | 0     | 원본                           |
    | 1     | 원본의 한영타 변환             |
    | 2     | 원본의 초성                    |
    | 3     | 원본의 초성의 한영타 변환      |
    | 4     | 중간 매칭                      |
    | 5     | 중간 매칭의 한영타 변환        |
    | 6     | 중간 매칭의 초성               |
    | 7     | 중간 매칭의 초성의 한영타 변환 |

- 예제 데이터 색인 결과

    | Key              | Relevance                              | Description                                        |
    | ---------------- | -------------------------------------- | -------------------------------------------------- |
    | 나이키             | 20 = 3(weight) + 17(conversion weight) | "나이키"의 원본                                    |
    | skdlzl           | 19 = 3(weight) + 16(conversion weight) | "나이키"의 한영타 변환                             |
    | ㄴㅇㅋ             | 18 = 3(weight) + 15(conversion weight) | "나이키"의 초성                                    |
    | sdz              | 17 = 3(weight) + 14(conversion weight) | "나이키"의 초성의 한영타 변환                      |
    | 운동화             | 19 = 2(weight) + 17(conversion weight) | "운동화"의 원본                                    |
    | dnsehdghk        | 18 = 2(weight) + 16(conversion weight) | "운동화"의 한영타 변환                             |
    | ㅇㄷㅎ             | 17 = 2(weight) + 15(conversion weight) | "운동화"의 초성                                    |		
    | deg              | 16 = 2(weight) + 14(conversion weight) | "운동화"의 초성의 한영타 변환                      |
    | 나이키 에어맥스      | 18 = 1(weight) + 17(conversion weight) | "나이키 에어맥스"의 원본                           |
    | skdlzl dpdjaortm | 17 = 1(weight) + 16(conversion weight) | "나이키 에어맥스"의 한영타 변환                    |
    | ㄴㅇㅋ ㅇㅇㅁㅅ      | 16 = 1(weight) + 15(conversion weight) | "'나이키 에어맥스"의 초성                          |
    | sdz ddat         | 15 = 1(weight) + 14(conversion weight) | "나이키 에어맥스"의 초성의 한영타 변환             |
    | 에어맥스           | 14 = 1(weight) + 13(conversion weight) | "나이키 에어맥스"의 중간 매칭                      |
    | dpdjaortm        | 13 = 1(weight) + 12(conversion weight) | "나이키 에어맥스"의 중간 매칭의 한영타 변환        |
    | ㅇㅇㅁㅅ           | 12 = 1(weight) + 11(conversion weight) | "나이키 에어맥스"의 중간 매칭의 초성               |
    | ddat             | 11 = 1(weight) + 10(conversion weight) | "나이키 에어맥스"의 중간 매칭의 초성의 한영타 변환 |

    - 사용자가 'ㅇ'을 입력했을때 '운동화'(relevance 19)가 '에어맥스'(relevance 14)보다 먼저 출력됩니다.

### ACL

ACL 설정 화면은 다음과 같습니다.

![img](http://static.toastoven.net/prod_autocomplete/acl-detail-ko-20200304.jpg)

**입력 형식**

- IP 형식으로 입력 가능합니다.
    - 예제) 202.179.177.21
- CIDR 형식으로 입력 가능합니다.
    - 예제) 202.179.177.0/24
- IP 또는 CIDR 을 여러 개 입력 가능합니다.
    - 예제) 202.179.177.21, 202.179.177.0/24
- all 일 경우 모두 매칭 됩니다.
- 값이 비어 있을 경우 모두 매칭 안됩니다.
- 허용, 거부 둘 다에 해당되면 거부됩니다.
- 허용, 거부 둘 다에 해당되지 않으면 거부됩니다.

## 클라이언트 예제 코드

다음은 파일 업로드 방식의 색인 예제 코드입니다.

### java

- dependency

```
compile group: 'org.apache.httpcomponents', name: 'httpclient', version: '4.5.6'
compile group: 'org.apache.httpcomponents', name: 'httpmime', version: '4.5.6'
```

- 색인(파일 업로드 방식)

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
			+ "	   \"input\": \"나이키\",\n"
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

- 색인(파일 업로드 방식)

```
<?php
    $documents = ""
      . "[\n"
      . "  {\n"
      . "    \"input\": \"나이키\",\n"
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
