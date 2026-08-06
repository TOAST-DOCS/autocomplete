<!-- pre-align:aligned sig=f2a39712547d -->

<a id="search-autocomplete-api-v20-guide"></a>
## Search > Autocomplete > API v2.0 Guide { #search-autocomplete-api-v20-guide }

This document describes the Autocomplete API v2.0 provided by Cloud Search.

<a id="common"></a>
## Common { #common }

<a id="api-endpoint"></a>
### API Endpoint { #api-endpoint }

<a id="api-endpoint-uri-information"></a>
#### URI Information

| Environment | URI                                              |
| ---- | ------------------------------------------------ |
| REAL | https://kr1-autocomplete.api.nhncloudservice.com |

<a id="api-endpoint-path-parameter-information"></a>
#### Path Parameter Information

| Name      | Description                    |
| --------- | ----------------------- |
| appKey    | Appkey issued from the console |
| serviceId | A random name for the user    |

<a id="authentication-and-authorization"></a>
### Authentication and Authorization { #authentication-and-authorization }

Appkey is required to use the Autocomplete API. The Appkey is included in the request URL to identify and specify a particular resource when making API calls.
For more information on checking and using Appkeys, please refer to the [Appkey](/nhncloud/en/public-api/appkey).

<a id="full-indexing"></a>
## Full indexing { #full-indexing }

If you run full indexing, previously indexed files will disappear.

You must proceed in the start-index-end order.

<a id="start"></a>
### 1. Start { #start }

**[Request]**

URI Information

| Method | URI                                                                            |
| ------ | ------------------------------------------------------------------------------ |
| POST   | /indexing/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/indexing/full/begin |

**[Response]**

Response Body

```
{}
```

<a id="indexing"></a>
### 2. Indexing { #indexing }

**[Request]**

URI Information

| Method | URI                                                                      |
| ------ | ------------------------------------------------------------------------ |
| POST   | /indexing/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/indexing/full |

BODY Information (example)

```
[
    {
        "id": "Monthly work plan",
        "input": [
            "Monthly work plan",
            "dnjfrks djqan rPghlr",
            "workplan",
            "djqan rPghlr",
            "Plan",
            "rPghlr"
        ],
        "weight": 1,
        "output": "Monthly work plan",
        "payload": [
            "{\"serviceType\":\"aa\",\"servicePlaceId\":\"bb\"}",
            "Ghana",
            "Dara",
            "{\"serviceType\":\"cc\",\"servicePlaceId\":\"dd\"}"
        ]
    }
]
```

**[Response]**

Response Body

```
{
    "id": 1
}
```

<a id="end"></a>
### 3. End { #end }

**[Request]**

URI Information

| Method | URI                                                                          |
| ------ | ---------------------------------------------------------------------------- |
| POST   | /indexing/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/indexing/full/end |

**[Response]**

Response Body

```
{}
```

<a id="cancel"></a>
### 4. Cancel { #cancel }

**[Request]**

URI Information

| Method | URI                                                                             |
| ------ | ------------------------------------------------------------------------------- |
| POST   | /indexing/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/indexing/full/cancel |

**[Response]**

Response Body

```
{}
```

<a id="update-the-index"></a>
## Update the index { #update-the-index }

The ID is required to update the index.

add modifies the document if it already exists, or add it if it doesn't.

delete deletes the document.

<a id="update-the-index-2"></a>
### 1. Update the index { #update-the-index-2 }

**[Request]**

URI Information

| Method | URI                                                                 |
| ------ | ------------------------------------------------------------------- |
| POST   | /indexing/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/indexing |

BODY Information (example)

```
[
    {
        "id": "id-1",
        "action": "add",
        "input": "Nike sneakers",
        "weight": 1
    },
    {
        "id": "id-2",
        "action": "delete"
    },
    {
        "id": "id-3",
        "action": "add",
        "input": "New Balance sneakers",
        "weight": 1
    }
]
```

**[Response]**

Response Body

```
{
    "id": 2
}
```

<a id="index-log"></a>
## Index log { #index-log }

<a id="view-the-index-log"></a>
### 1. View the index log { #view-the-index-log }

**[Request]**

URI Information (example)

| Method | URI                                                                          |
| ------ | ---------------------------------------------------------------------------- |
| GET    | /indexing/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/indexing_log?id=1 |

Parameter Information

| Name | Description    |
| ---- | ------- |
| id   | Indexing ID |

**[Response]**

Response body (example)

```
{
    "request_time": "2024-02-15T17:18:56",
    "file_name": "payload-1.json",
    "file_size": 1210,
    "status": 4
}
```

<a id="search"></a>
## Search { #search }

You can search for input.

<a id="search-2"></a>
### 1. Search { #search-2 }

**[Request]**

URI Information (example)

| Method | URI                                                                                       |
| ------ | ----------------------------------------------------------------------------------------- |
| GET    | /indexing/v2.0/appkeys/{{appKey}}/serviceids/{{serviceId}}/autocomplete?count=10&query=new |

Parameter Information

| Name  | Description             |
| ----- | ---------------- |
| count | Number of results (required) |
| query | Query (required)      |

**[Response]**

Response body (example)

```
{
    "collections": [
        {
            "index": 0,
            "items": [
                [
                    "adidas shoes"
                ],
                [ 
                    "Nike shoes"
                ]
            ],
            "title": ""
        }
    ],
    "query": [
        "god",
        "tls"
    ],
    "ver": "v2.0"
}
```
