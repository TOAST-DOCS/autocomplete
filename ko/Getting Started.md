## Upcomming Products > Autocomplete > Getting Started

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
        * 영문 소문자, 숫자 및 일부 특수문자만 가능합니다.
        * 사용 가능한 특수 문자
        ```
        '~' '@' '$' '&' '(' ')' ':' '_' '-' '.'
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
          $ curl -XPOST 'http://alpha-api-autocomplete.cloud.toast.com/indexing/v1.0/appkeys/3PrEhyNmfipIHMkZ/domains/test/indexing' -H 'Content-Type:multipart/form-data; charset=UTF-8' -F 'file=@documents.json'
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
            curl -i -XGET 'https://alpha-api-autocomplete.cloud.toast.com/indexing/v1.0/appkeys/3PrEhyNmfipIHMkZ/domains/test/indexing_log?id=1'
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
    $ curl -G -XGET 'http://alpha-api-autocomplete.cloud.toast.com/autocomplete/v1.0/appkeys/3PrEhyNmfipIHMkZ/domains/test/autocomplete?count=10' --data-urlencode query='나'
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
