## Upcomming Products > Autocomplete > Developer's guide

## 기능 설명

### 부가 정보 출력
* 자동완성 출력시 부가 정보(이미지 URL, 카테고리, etc...)를 같이 출력하는 기능합니다.
* 색인 파일 생성 (data/documents.json)
    ```json
    [
      {
        "input": "나이키",
        "payload": ["http://image.nhnent.com/images/nike.jpg", "브랜드>스포츠"],
        "weight": 2
      },
      {
        "input": "아디다스",
        "payload": ["http://image.nhnent.com/images/adidas.jpg", "브랜드>스포츠"],
        "weight": 2
      }
    ]
    ```
    * payload 에 출력하고 싶은 부가 정보를 입력합니다.
<br>    
* 색인
    ![](http://static.toastoven.net/prod_autocomplete/indexing.png?)
<br>
* 자동완성
    ![](http://static.toastoven.net/prod_autocomplete/suggest-extra_info.png)
    1. 이미지 URL과 카테고리 정보가 출력됩니다.

### Input/Output을 다르게 설정
* "나이키"를 입력 했을때 "Nike"가 출력되도록 하는 기능입니다.
* 색인 파일 생성 (data/documents.json)
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
* 색인
    ![](http://static.toastoven.net/prod_autocomplete/indexing.png)
<br>
* 자동완성
    ![](http://static.toastoven.net/prod_autocomplete/input-ouput.png)
    1. "나"를 입력했을때
    2. "Nike"가 출력됩니다.

### 초성 검색
* "ㄴㅇㅋ"를 입력했을때 "나이키"가 출력되도록 하는 기능입니다.
* 색인 파일 생성 (data/documents.json)
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
* 색인
    ![](http://static.toastoven.net/prod_autocomplete/indexing.png)
<br>
* 자동완성
    ![](http://static.toastoven.net/prod_autocomplete/suggest-cho.png)
    1. "ㄴㅇㅋ"를 입력했을때
    2. "나이키"가 출력 됩니다.

### Multi domain
* 2개 이상 domain 의 자동완성 결과를 한번의 자동완성 API 요청으로 출력되도록 하는 기능입니다.
    * 예를 들어 브랜드와 카테고리 자동완성을 한번의 API 요청으로 출력할때 사용합니다.
* brand 도메인 생성
    ![](http://static.toastoven.net/prod_autocomplete/domain_create-brand.png)
* brand 색인 파일 생성 (data/brand.json)
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
* brand 색인
    ![](http://static.toastoven.net/prod_autocomplete/indexing-brand.png?)
<br>    
* category 도메인 생성
    ![](http://static.toastoven.net/prod_autocomplete/domain_create-category.png)
<br>
* category 색인 파일 생성 (data/category.json)
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

* category 색인
    ![](http://static.toastoven.net/prod_autocomplete/indexing-category.png)
* 자동완성
    ```
    [admin@NHNEnt:~]$ curl -G -XGET 'http://alpha-api-autocomplete.cloud.toast.com/suggest/v1.0/appkeys/rjmIWV4TQuTaxvAc/domains/brand,category/suggest?count=10' --data-urlencode query='나'
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
    * API 호출시 domain/brand,category 로 요청했습니다.
    * API 응답에 index 0 는 brand, index 1 은 category 자동완성 결과가 출력 됩니다.

## 상세 설명

### ACL
![](http://static.toastoven.net/prod_search/detail-acl.png?)

* 입력형식
    * IP 형식으로 입력 가능합니다.
        * 예제) 202.179.177.21
    * CIDR 형식으로 입력가능합니다.
        * 예제) 202.179.177.0/24
    * IP 또는 CIDR 을 여러개 입력 가능합니다.
        * 예제) 202.179.177.21, 202.179.177.0/24
    * all 일 경우 모두 매칭됩니다.
    * 값이 비어 있을 경우 모두 매칭안됩니다.  
* Allow, Deny 둘다에 매칭이 될 경우 Deny 됩니다.
* Allow, Deny 둘다에 매칭이 안될 경우 Deny 됩니다.    
