## Upcomming Products > Autocomplete > Getting Started

## 서비스 활성화
Autocomplete 서비스를 사용하기 위해서는 Console에서 [Upcoming Products] > [Autocomplete] 을 선택한 후 [상품이용] 버튼을 클릭하여 서비스를 활성화시킵니다.
![](http://static.toastoven.net/prod_autocomplete/product-use-01.png)
![](http://static.toastoven.net/prod_autocomplete/product-use-02.png?)

1. "Upcoming Proucts"를 클릭합니다.
2. "Autocomplete"를 클릭합니다.
3. "상품이용" 버튼을 클릭합니다.
<br>

## 기본 사용법

### 도메인 생성
* 도메인 생성 방법
    ![](http://static.toastoven.net/prod_autocomplete/domain_create_procedure_01.png?)
    1. "도메인 생성" 버튼을 클릭합니다.
    2. 도메인 이름을 입력합니다.
        * 영문, 숫자 및 특수문자만 가능합니다.
        * 사용 불가 문자
        ```
        "\", "/", "*", "?", "\"", "<", ">", "|", " ", ",", "#", "^", "!"
        ```
    3. "저장" 버튼을 클릭합니다.    
<br>
* 도메인 생성 결과
    ![](http://static.toastoven.net/prod_autocomplete/domain_create_result.png?)
    1. 생성된 도메인(test)를 클릭합니다.
<br>
* 제약 사항
    * 하나의 appKey(사용자) 당 도메인은 최대 32개 까지 생성 가능합니다.

### 색인
* 색인할 파일 생성
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
<br>
* 색인 방법
    ![](http://static.toastoven.net/prod_autocomplete/indexing_procedure_01.png)
    ![](http://static.toastoven.net/prod_autocomplete/indexing_procedure_02.png??)
    1. "색인" 탭을 클릭합니다.
    2. "파일 선택" 버튼을 클릭합니다.
    3. 색인할 파일을 선택합니다.
    4. "열기" 버튼을 클릭합니다.  
    5. 색인 명령어가 Rest API 로 출력됩니다.
    6. "색인" 버튼을 클릭합니다.
<br>
* Rest API
    * 아래와 같이 Rest API를 사용 가능합니다.
    ```
    [admin@NHNEnt:data]$ curl -XPOST 'http://alpha-api-autocomplete.cloud.toast.com/indexing/v1.0/appkeys/rjmIWV4TQuTaxvAc/domains/test/indexing' -H 'Content-Type:multipart/form-data; charset=UTF-8' -F 'file=@documents.json'
    {
      "result" : "success",
      "code" : 1
    }
    ```

### 자동완성
* 자동완성 방법
    ![](http://static.toastoven.net/prod_autocomplete/autocomplete_procedure.png)
    1. "자동완성" 탭을 클릭합니다.
    2. 검색할 단어를 입력합니다.
    3. 출력 개수를 지정합니다.
    4. 자동완성 Rest API 입니다.
    5. 자동완성 결과가 출력됩니다.   
<br>    
* Rest API
    * 아래와 같이 Rest API를 사용 가능합니다.
    ```
    [admin@NHNEnt:~]$ curl -G -XGET 'http://alpha-api-autocomplete.cloud.toast.com/suggest/v1.0/appkeys/rjmIWV4TQuTaxvAc/domains/test/suggest?count=10' --data-urlencode query='나'
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
