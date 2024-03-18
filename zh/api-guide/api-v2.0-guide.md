## Search > Autocomplete > API v2.0 Guide

This document describes the Autocomplete API v2.0 provided by NHN Cloud Search.

## Common

**[Request]**

URI Information

| Environment | URI                                              |
| ---- | ------------------------------------------------ |
| REAL | https://kr1-autocomplete.api.nhncloudservice.com |

Path Parameter Information

| Name      | Description                    |
| --------- | ----------------------- |
| appKey    | Appkey issued from the console |
| serviceId | A random name for the user    |

## Full indexing

If you run full indexing, previously indexed files will disappear.

You must proceed in the start-index-end order.

### 1. Start

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

### 2. Indexing

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

### 3. End

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

### 4. Cancel

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

## Update the index

The ID is required to update the index.

add modifies the document if it already exists, or add it if it doesn't.

delete deletes the document.

### 1. Update the index

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

## Index log

### 1. View the index log

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

## Search

You can search for input.

### 1. Search

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
