## Search > Autocomplete > Console User Guide

## Prerequisites

- The appkey, 'PyVTgcSXJpA3e5U7', within document, is different for each user.

## Getting Started

First, enable the Automcomplete Service.

1. Go to **NHN Cloud Console** and click **Select Services**.

2. Click **Autocomplete**.

![img](http://static.toastoven.net/prod_autocomplete/product-use-02-en-20210506.jpg)

 Do as follows to check if service is enabled:

1. Click **Search** on the left menu on **NHN Cloud Console**.

2. If you can find **Autocomplete**, the service has been enabled.

![img](http://static.toastoven.net/prod_autocomplete/product-use-03-en-20210506.jpg)

## Basic Usage

### 1. Creating Services
1. Click **Create Services**.

2. Enter Service ID on **Create Services**.
    - Only English in lowercase, numbers, as well as \_(underscore) and -(hypen) are available.
    - Cannot start with a number, \_(underscore) or -(hypen).
    - At least two characters are required.

3. Click **Save**.

![img](http://static.toastoven.net/prod_autocomplete/domain_create_procedure-en-20210506.jpg)

Check the result of service creation.
1. Click Service ID which is created (test).

![img](http://static.toastoven.net/prod_autocomplete/domain_create_result-en-20210506.jpg)

### 2. Indexing

Do as follows to create and index files.

**Create Index Files**

  - Create indexing request files in the format of example as below
  - Create files encoded in UTF-8.
      - Save the file on Windows memo, by specifying UTF-8 for encoding.
  - It was created in the name of data/documents.json in the example.
  - The maximum file size is 10 MB.

```
[
  {
    "input": "Nike",
    "weight": 3
  },
  {
    "input": "Nike sneakers",
    "weight": 2
  },
  {
    "input": "Sneakers",
    "weight": 1
  }
]
```

**How to indexing**

1. Click **Indexing**.

2. Click **Select File**.

3. Select a file to indexing.

4. Press **Open**.

5. Indexing command comes as REST API.

6. Click **Indexing**.

7. Click **Refresh**.

8. Check indexing results.

![img](http://static.toastoven.net/prod_autocomplete/indexing_procedure-01-en-20230905.jpg)  
![img](http://static.toastoven.net/prod_autocomplete/indexing_procedure-02-en-20230905.jpg)

**Caution for Index**

When index is requested, previous data are all deleted and replaced by new data.



**REST API**

  - Indexing API

    - Request

        - By File Uploading

            ```
            curl -XPOST 'https://kr1-autocomplete.api.nhncloudservice.com/indexing/v2.0/appkeys/PyVTgcSXJpA3e5U7/serviceids/test/indexing?split=true&koreng=true&chosung=false' -H 'Accept-Language:en' -H 'Content-Type:multipart/form-data; charset=UTF-8' -F 'file=@documents.json'
            ```

        - By Payload

            ```
            curl -i -XPOST 'https://kr1-autocomplete.api.nhncloudservice.com/indexing/v2.0/appkeys/PyVTgcSXJpA3e5U7/serviceids/test/indexing?split=true&koreng=true&chosung=false' -H 'Accept-Language:en' -H 'Content-Type:application/json; charset=UTF-8' -d '
            [
              {
                "input": "Nike",
                "weight": 3
              },
              {
                "input": "Nike Sneakers",
                "weight": 2
              },
              {
                "input": "Sneakers",
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

  - Indexing Result Check API

    - Request

        ```
        curl -i -XGET 'https://kr1-autocomplete.api.nhncloudservice.com/indexing/v2.0/appkeys/PyVTgcSXJpA3e5U7/serviceids/test/indexing_log?id=1' -H 'Accept-Language:en'
        ```

        - id 1 refers to the ID for Response of Index API in the above.

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
            - 1: Waiting
            - 2: Ignored
            - 3: Progressing
            - 4: Successful  
            - 5: Failed
			- 6: Canceled

### 3. Autocomplete

**How to Autocomplete**

1. Click **Autocomplete**.

2. Enter a word to search.

3. Specify output count.

4. Autocomplete REST API shows.

5. Result of autocomplete comes out. .

![img](http://static.toastoven.net/prod_autocomplete/autocomplete_procedure-en-20231030.jpg)

**REST API**

  - Request

    ```
    curl -G -XGET 'https://kr1-autocomplete.api.nhncloudservice.com/autocomplete/v2.0/appkeys/PyVTgcSXJpA3e5U7/serviceids/test/autocomplete?count=10' -H 'Accept-Language:en' --data-urlencode query='ni'
    ```

  - Response

    ```
    {
      "collections" : [
        {
          "index" : 0,
          "items" : [
            [
              "nikie"
            ],
            [
              "nike sneakers"
            ] 
          ],
          "title" : ""
        }
      ],
      "query" : [
        "ni",
        "ㅜㅑ"
      ],
      "ver" : "2.0"
    }
    ```

### 4. ACL

IPs may be restricted for equipment which may call indexing and autocomplete REST APIs.
Testing on console does not require ACL setting.

**How to Set ACL**

1. Click **ACL**.

2. The example regards to indexing which is available only when the request IP is 202.179.177.21.

3. In the example, request for autocomplete can be made from all IPs.   

4. Click **Save**.

![img](http://static.toastoven.net/prod_autocomplete/acl_procedure-en-20200304.jpg)



## Feature Details  

### Middle Match

**Index**

To test, index data as below:

```
[
  {
    "input": "Nike sneakers",
    "weight": 2
  },
  {
    "input": "Adidas sneakers",
    "weight": 1
  }
]
```

Select **Middle-match** for indexing.

![img](http://static.toastoven.net/prod_autocomplete/infix-indexing-en-20230905.jpg)

**Autocomplete**

1. Enter **Workout **.

2. You can find in the middle **Nike Sneakers** starting with **Workout**

![img](http://static.toastoven.net/prod_autocomplete/infix-suggest-en-20231030.jpg)

### Output of Additional Information

**Index**

To test, index data as below:

```
[
  {
    "input": "Nike",
    "payload": [
      "https://image.nhnent.com/images/nike.jpg",
      "Brand>Sports"
    ],
    "weight": 2
  },
  {
    "input": "Adidas",
    "payload": [
      "https://image.nhnent.com/images/adidas.jpg",
      "Brand>Sports"
    ],
    "weight": 1
  }
]
```

Enter additional information you need as output for payload.  

**Autocomplete**

1. The additional information input (image URL and category) comes as output.

![img](http://static.toastoven.net/prod_autocomplete/suggest-payload-en-20231030.jpg)



### Different Settings for Input/Output

**Index**

To test, index data as below:

```
[
  {
    "input": "Shoes",
    "output": "Sneakers",
    "weight": 2
  },
  {
    "input": "Boots",
    "output": "Sneakers",
    "weight": 1
  }
]
```

**Autocomplete**

1. Enter 'sh'.

2. 'Sneakers' comes as output.

![img](http://static.toastoven.net/prod_autocomplete/suggest-output-en-20231030.jpg)


### Multiple Services

Autocomplete results of more than two services come with one-time request for autocomplete API. For instance, the autocomplete for brand and category can be made available with one-time API request.    

**Creating Brand Services**

Click **Create Services**, enter ID on a popup of **Create Services**, and click **Create**.  

![img](http://static.toastoven.net/prod_autocomplete/domain_create-brand-en-20200304.jpg)

**Brand Index**

To test, index data as below:

```
[
  {
    "input": "Nike",
    "weight": 2
  },
  {
    "input": "Adidas",
    "weight": 1
  }
]
```

**Creating Category Services **

Click **Create Services** , enter ID on a popup for **Create Services**, and click **Create**.

![img](http://static.toastoven.net/prod_autocomplete/domain_create-category-en-20200304.jpg)

**Category Index**

To test, index data as below:

```
[
  {
    "input": "Activewear",
    "weight": 2
  },
  {
    "input": "Outerwear",
    "weight": 1
  }
]
```

**Autocomplete**

- Request

    Requested with 'serviceids/brand,category' to call APIs.

    ```
    curl -G -XGET 'https://kr1-autocomplete.api.nhncloudservice.com/autocomplete/v2.0/appkeys/PyVTgcSXJpA3e5U7/serviceids/brand,category/autocomplete?count=10' -H 'Accept-Language:en' --data-urlencode query='a'
    ```		

- Response

    The API response shows autocomplete results for brand at index 0 and category at index 1.  

    ```
    {
      "collections" : [
        {
          "index" : 0,
          "items" : [
            [
              "adidas"
            ]
          ],
          "title" : ""
        },
        {
          "index" : 1,
          "items" : [
            [
              "actiewear"
            ]
          ],
          "title" : ""
        } 
      ],
      "query" : [
        "a",
        "ㅁ"
      ],
      "ver" : "2.0"
    }
    ```

### 대용량 데이터 색인
기본 색인은 입력할 수 있는 데이터 크기가 10MB로 제한되어 있습니다.
10MB를 초과하는 데이터를 입력할 때는 Full indexing API를 사용합니다.

- Full indexing 시작
    ```
    curl -i -XPOST 'https://kr1-autocomplete.api.nhncloudservice.com/indexing/v2.0/appkeys/PyVTgcSXJpA3e5U7/serviceids/test/indexing/full/begin'
    ```
    - 새로운 index(저장소)가 생성됩니다.
    - Full indexing을 반영하기 전까지는 기존 index로 서비스됩니다.
- Full indexing 요청
    ```
    curl -XPOST 'https://kr1-autocomplete.api.nhncloudservice.com/indexing/v2.0/appkeys/PyVTgcSXJpA3e5U7/serviceids/test/indexing/full?split=true&koreng=true&chosung=true' -H 'Content-Type:multipart/form-data; charset=UTF-8' -F 'file=@documents-001.json'
    ```
    - documents-002.json, documents-003.json 등 여러 번 색인 요청을 합니다.		
- Full indexing 반영
    ```
    curl -i -XPOST 'https://kr1-autocomplete.api.nhncloudservice.com/indexing/v2.0/appkeys/PyVTgcSXJpA3e5U7/serviceids/test/indexing/full/end'
    ```
    - 색인된 데이터를 서비스에 반영합니다.		
- Full indexing 취소
    ```
    curl -i -XPOST 'https://kr1-autocomplete.api.nhncloudservice.com/indexing/v2.0/appkeys/PyVTgcSXJpA3e5U7/serviceids/test/indexing/full/cancel'
    ```
    - 색인이 진행 중일 때는 동작하지 않습니다.

### 색인 업데이트

데이터를 추가/수정/삭제할 때는 Incremental indexing API를 사용합니다.

**1. 테스트를 위한 데이터 입력**

```
curl -XPOST 'https://kr1-autocomplete.api.nhncloudservice.com/indexing/v2.0/appkeys/PyVTgcSXJpA3e5U7/serviceids/test/indexing?split=true&koreng=true&chosung=false' -H 'Accept-Language:en' -H 'Content-Type:application/json; charset=UTF-8' -d '
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
]'
```

- Incremental indexing을 수행하기 위해서는 id를 반드시 입력해야 합니다.

**2. Incremental indexing**

```
curl -XPOST 'https://kr1-autocomplete.api.nhncloudservice.com/indexing/v2.0/appkeys/PyVTgcSXJpA3e5U7/serviceids/test/indexing/incremental?split=true&koreng=true&chosung=false' -H 'Accept-Language:en' -H 'Content-Type:application/json; charset=UTF-8' -d '
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
]'
```

- action
    - add: 기존에 문서가 존재하면 수정, 존재하지 않으면 추가됩니다.
        - 위의 예제에서 "id-1"은 수정, "id-3"은 추가됩니다.
    - delete: 해당 문서를 삭제합니다.

## Guide Details

### Priority of Output

In the case of the following index files, when user enters 'no', the output shows in the order of 'notebook', 'notepad', 'note cards'.

```
[
  {
    "input": "notebook",
    "weight": 3
  },
  {
    "input": "notepad",
    "weight": 2
  },
  {
    "input": "note cards",
    "weight": 1
  }
]
```
- Words starting with 'no' come in the order of higher weights.

### ACL

ACL can be set on a page like this:

![img](http://static.toastoven.net/prod_autocomplete/acl-detail-en-20200304.jpg)

**Input Formats**

- Available in the IP format.
    - Example) 202.179.177.21
- Available in the CIDR format.
    - Example) 202.179.177.0/24
- Multiple inputs of IP or CIDR are available.
    - Example) 202.179.177.21, 202.179.177.0/24
- All matched for all.
- All dis-matched when value is empty.
- Rejected if applied both to Allow and Reject.
- Rejected if not applied either to Allow or Reject.  

## Client Example Codes

Following shows the file-uploading type index example codes.   

### java

- dependency

```
compile group: 'org.apache.httpcomponents', name: 'httpclient', version: '4.5.6'
compile group: 'org.apache.httpcomponents', name: 'httpmime', version: '4.5.6'
```

- Index (by file uploading)

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
				.post("https://kr1-autocomplete.api.nhncloudservice.com/indexing/v2.0/appkeys/PyVTgcSXJpA3e5U7/serviceids/test/indexing?split=true&koreng=true&chosung=false")
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

- Index (by file uploading)

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
    curl_setopt($ch, CURLOPT_URL,"https://kr1-autocomplete.api.nhncloudservice.com/indexing/v2.0/appkeys/PyVTgcSXJpA3e5U7/serviceids/test/indexing?split=true&koreng=true&chosung=false");
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
