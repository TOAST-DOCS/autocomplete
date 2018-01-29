## Search > Autocomplete > 콘솔 사용 가이드

## 서비스 활성화
* Autocomplete 서비스를 활성화 하기 위해서 Console로 이동합니다.
* 활성화 방법
    ![](http://static.toastoven.net/prod_autocomplete/product-use-02.png)
    1. "서비스 선택"을 클릭합니다.
    2. "Autocomplete"을 클릭해서 서비스를 활성화 합니다.
<br>
* 활성화 확인
    ![](http://static.toastoven.net/prod_autocomplete/product-use-03.png)
    1. "Search" 클릭합니다.
    2. "Autocomplete"가 노출되면 활성화된 것입니다.
<br>

## 기본 사용법

### 도메인 생성
* 도메인 생성 방법
    ![](http://static.toastoven.net/prod_autocomplete/domain_create_procedure.png?)
    1. "도메인 생성" 버튼을 클릭합니다.
    2. 도메인 이름을 입력합니다.
        * 영문 소문자, 숫자 및 일부 특수 문자만 가능합니다.
        * 사용 가능한 특수 문자
        ```
        '~' '@' '$' '&' '(' ')' ':' '_' '-'
        ```        
    3. "저장" 버튼을 클릭합니다.    
<br>
* 도메인 생성 결과
    ![](http://static.toastoven.net/prod_autocomplete/domain_create_result.png?)
    1. 생성된 도메인(test)를 클릭합니다.
<br>

### 색인
* 색인할 파일 생성
    * <span style="color:red">색인할 파일은 UTF-8 로 생성해야 합니다.</span>
        * Windows 메모장에서 파일 저장시 인코딩을 UTF-8 로 지정해서 저장합니다.
    * 예제에서는 data/documents.json 이름으로 생성했습니다.
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
    * 최대 파일 사이즈는 160Mb 입니다.
<br>
* 색인 방법
    ![](http://static.toastoven.net/prod_autocomplete/indexing_procedure_01.png)
    ![](http://static.toastoven.net/prod_autocomplete/indexing_procedure_02.png??????????????????)
    1. "색인" 탭을 클릭합니다.
    2. "파일 선택" 버튼을 클릭합니다.
    3. 색인할 파일을 선택합니다.
    4. "열기" 버튼을 클릭합니다.  
    5. 색인 명령어가 Rest API 로 출력됩니다.
    6. "색인" 버튼을 클릭합니다.
    7. "Refresh" 버튼을 클릭합니다.
    8. 색인 결과를 확인합니다.
<br>
* Rest API
    * 색인 API
       * Request
          ```
          $ curl -XPOST 'http://alpha-api-autocomplete.cloud.toast.com/indexing/v1.0/appkeys/3PrEhyNmfipIHMkZ/serviceids/test/indexing' -H 'Content-Type:multipart/form-data; charset=UTF-8' -F 'file=@documents.json'
          ```
       * Response
          ```
          {
            "id" : 1
          }
          ```    
    * 색인 결과 확인 API
        * Request
            ```
            curl -i -XGET 'https://alpha-api-autocomplete.cloud.toast.com/indexing/v1.0/appkeys/3PrEhyNmfipIHMkZ/serviceids/test/indexing_log?id=1'
            ```
            * id 1은 위의 색인 API Response 의 id 입니다.
        * Response
            ```
            {
              "request_time" : "2017-10-23T12:36:43",
              "file_name" : "documents.json",
              "file_size" : "114",
              "status" : "4"
            }
            ```
            * status
                * 1 : 대기중
                * 2 : 무시됨 (필드 설정 변경 이전의 색인 요청은 무시됨, 색인 요청 이후에 필드 설정 변경을 했어도 시스템 내부 스케쥴링에 의해 필드 설정 변경 이후에 색인 요청이 수행될 수 있음)
                * 3 : 진행중
                * 4 : 성공
                * 5 : 실패

### 자동완성
* 자동완성 방법
    ![](http://static.toastoven.net/prod_autocomplete/autocomplete_procedure.png?)
    1. "자동완성" 탭을 클릭합니다.
    2. 검색할 단어를 입력합니다.
    3. 출력 개수를 지정합니다.
    4. 자동완성 Rest API 입니다.
    5. 자동완성 결과가 출력됩니다.   
<br>    
* Rest API
    * 아래와 같이 Rest API를 사용 가능합니다.
    * Request    
    ```
    $ curl -G -XGET 'http://alpha-api-autocomplete.cloud.toast.com/autocomplete/v1.0/appkeys/3PrEhyNmfipIHMkZ/serviceids/test/autocomplete?count=10' --data-urlencode query='나'
    ```
    * Response
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

### ACL
* ACL 설정 방법
    ![](http://static.toastoven.net/prod_autocomplete/acl_procedure.png???)
    1. "ACL" 탭을 클릭합니다.
    2. 색인 요청 IP 주소가 202.179.177.21 인 경우만 색인이 가능하도록 설정한 예제입니다.
    3. 자동완성 요청은 모든 IP 에서 가능하도록 설정한 예제입니다.
    4. "저장" 버튼을 클릭합니다.  
<br>

## 기능 가이드

### 부가 정보 출력
* 자동완성 출력시 부가 정보(이미지 URL, 카테고리, etc...)를 같이 출력하는 기능합니다.
* 색인 파일 생성 (data/documents.json)
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
    * payload 에 출력하고 싶은 부가 정보를 입력합니다.
<br>   
* 색인
    ![](http://static.toastoven.net/prod_autocomplete/indexing.png?)
<br>
* 자동완성
    ![](http://static.toastoven.net/prod_autocomplete/suggest-extra_info.png?)
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
    ![](http://static.toastoven.net/prod_autocomplete/input-output.png?)
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
    ![](http://static.toastoven.net/prod_autocomplete/suggest-cho.png???)
    1. "ㄴㅇㅋ"를 입력했을때
    2. "나이키"가 출력 됩니다.

### 한타<->영타 자동 변화
* "나이키"의 영타 "skdlzl"를 입력했을때 "나이키"가 출력되도록 하는 기능입니다.
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
    ![](http://static.toastoven.net/prod_autocomplete/suggest-haneng.png?)
    1. "나이"의 영타인 ""skdl"를 입력했을때
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
    [admin@NHNEnt:~]$ curl -G -XGET 'http://alpha-api-autocomplete.cloud.toast.com/autocomplete/v1.0/appkeys/3PrEhyNmfipIHMkZ/domains/brand,category/autocomplete?count=10' --data-urlencode query='나'
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

### 출력 우선 순위
* 색인 파일이 아래와 같을경우 사용자가 'ㄴ' 을 입력하면 "노트북", "나이키", "남성상의" 순으로 출력됩니다.
    * 'ㄴ'으로 시작하는 단어중 weight 가 높은 순으로 출력됩니다.
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

### ACL
![](http://static.toastoven.net/prod_autocomplete/detail-acl.png?)

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
