## Search > Autocomplete > Console User Guide

## Prerequisites

- The host name, 'api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com', within document, may be different for each user.
- The appkey, '7IkFjTvxA8zwfL8e', within document, is different for each user.

## Getting Started

First, enable the Automcomplete Service.

1. Go to **TOAST Console** and click **Select Services**.

2. Click **Autocomplete**.

![img](http://static.toastoven.net/prod_autocomplete/product-use-02-en-20200304.jpg)

 Do as follows to check if service is enabled:

1. Click **Search** on the left menu on **TOAST Console**.

2. If you can find **Autocomplete**, the service has been enabled.

![img](http://static.toastoven.net/prod_autocomplete/product-use-03-en-20200304.jpg)

## Basic Usage

### 1. Creating Services
1. Click **Create Services**.

2. Enter Service ID on **Create Services**.
    - Only English in lowercase, numbers, as well as \_(underscore) and -(hypen) are available.
    - Cannot start with a number, \_(underscore) or -(hypen).
    - At least two characters are required.

3. Click **Save**.

![img](http://static.toastoven.net/prod_autocomplete/domain_create_procedure-en-20200304.jpg)

Check the result of service creation.
1. Click Service ID which is created (test).

![img](http://static.toastoven.net/prod_autocomplete/domain_create_result-en-20200304.jpg)

### 2. Indexing

Do as follows to create and index files.

**Create Index Files**

  - Create indexing request files in the format of example as below
  - Create files encoded in UTF-8.
      - Save the file on Windows memo, by specifying UTF-8 for encoding.
  - It was created in the name of data/documents.json in the example.
  - The maximum file size is 32 MB.

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

![img](http://static.toastoven.net/prod_autocomplete/indexing_procedure-01-en-20200304.jpg)  
![img](http://static.toastoven.net/prod_autocomplete/indexing_procedure-02-en-20200304.jpg)

**Caution for Index**

When index is requested, previous data are all deleted and replaced by new data.  색



**REST API**

  - Indexing API

    - Request

        - By File Uploading

            ```
            curl -XPOST 'http://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/indexing/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/test/indexing?split=true&koreng=true&chosung=false' -H 'Content-Type:multipart/form-data; charset=UTF-8' -F 'file=@documents.json'
            ```

        - By Payload

            ```
            curl -i -XPOST 'http://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/indexing/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/test/indexing?split=true&koreng=true&chosung=false' -H 'Content-Type:application/json; charset=UTF-8' -d '
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
        curl -i -XGET 'https://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/indexing/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/test/indexing_log?id=1'
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

### 3. Autocomplete

**How to Autocomplete**

1. Click **Autocomplete**.

2. Enter a word to search.

3. Specify output count.

4. Autocomplete REST API shows.

5. Result of autocomplete comes out. .

![img](http://static.toastoven.net/prod_autocomplete/autocomplete_procedure-en-20200304.jpg)

**REST API**

  - Request

    ```
    curl -G -XGET 'http://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/autocomplete/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/test/autocomplete?count=10' --data-urlencode query='ni'
    ```

  - Response

    ```
    {
      "collections" : [ {
        "index" : 0,
        "items" : [ [ "nikie" ], [ "nike sneakers" ] ],
        "title" : ""
      } ],
      "query" : [ "ni", "ㅜㅑ" ],
      "ver" : "1.0"
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

![img](http://static.toastoven.net/prod_autocomplete/infix-indexing-en-20200304.jpg)

**Autocomplete**

1. Enter **Workout **.

2. You can find in the middle **Nike Sneakers** starting with **Workout**

![img](http://static.toastoven.net/prod_autocomplete/infix-suggest-en-20200304.jpg)

### Output of Additional Information

**Index**

To test, index data as below:

```
[
  {
    "input": "Nike",
    "payload": ["http://image.nhnent.com/images/nike.jpg", "Brand>Sports"],
    "weight": 2
  },
  {
    "input": "Adidas",
    "payload": ["http://image.nhnent.com/images/adidas.jpg", "Brand>Sports"],
    "weight": 1
  }
]
```

Enter additional information you need as output for payload.  

**Autocomplete**

1. The additional information input (image URL and category) comes as output.

![img](http://static.toastoven.net/prod_autocomplete/suggest-payload-en-20200304.jpg)



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

![img](http://static.toastoven.net/prod_autocomplete/suggest-output-en-20200304.jpg)


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

Requested with 'serviceids/brand,category' to call APIs.

The API response shows autocomplete results for brand at index 0 and category at index 1.  

```
curl -G -XGET 'http://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/autocomplete/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/brand,category/autocomplete?count=10' --data-urlencode query='a'
{
  "collections" : [ {
    "index" : 0,
    "items" : [ [ "adidas" ] ],
    "title" : ""
  }, {
    "index" : 1,
    "items" : [ [ "actiewear" ] ],
    "title" : ""
  } ],
  "query" : [ "a", "ㅁ" ],
  "ver" : "1.0"
}
```

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

**conversion_weights**

The output order can be adjusted for original, middled match autocomplete.  

- Example

    ```
    curl -i -XPOST 'http://api-7ab1617e2df0f1d1-autocomplete.cloud.toast.com/indexing/v1.0/appkeys/7IkFjTvxA8zwfL8e/serviceids/test/indexing?split=true&koreng=true&chosung=true&conversion_weights=17,0,0,0,13,0,0,0' -H 'Content-Type:application/json; charset=UTF-8' -d '
    [
      {
        "input": "nike airmax",
        "weight": 2
      },
      {
        "input": "airpods",
        "weight": 1
      }			
    ]'
    ```

    - Numbers are specified by "17,0,0,0,13,0,0,0" for conversion_weights.

- Significance of Each Index of conversion_weights

    | Index | Description                                                  |
    | ----- | ------------------------------------------------------------ |
    | 0     | Original                                                     |
    | 1     | Not available for english                                    |
    | 2     | Not available for english                                    |
    | 3     | Not available for english                                    |		
    | 4     | Middle match                                                 |
    | 5     | Not available for english                                    |		
    | 6     | Not available for english                                    |		
    | 7     | Not available for english                                    |		

- Index results of example data  

    | Key              | Relevance                              | Description                                                  |
    | ---------------- | -------------------------------------- | ------------------------------------------------------------ |
    | nike airmax       | 19 = 2(weight) + 17(conversion weight) | Original of "nike airmax"                                     |
    | airmax            | 15 = 2(weight) + 13(conversion weight) | Middle match of "nike airmax"                                 |
    | airpods           | 18 = 1(weight) + 17(conversion weight) | Original of "airpods"                                          |

    - By entering 'ai', 'airpods'(relevance 18) comes before 'airmax'(relevance 15).

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
