## Search > Autocomplete > 콘솔 사용 가이드

## 문서 설명
* 문서 내의 호스트명 "alpha-api-autocomplete.cloud.toast.com"는 사용자별로 다를 수 있습니다.
* 문서 내의 앱키 "3PrEhyNmfipIHMkZ"는 사용자별로 다를 수 있습니다.

## 서비스 활성화
* Autocomplete 서비스를 활성화하기 위해서 Console로 이동합니다.
* 활성화 방법
    ![](http://static.toastoven.net/prod_autocomplete/product-use-02-20180724.png)
    1. "서비스 선택"을 클릭합니다.
    2. "Autocomplete"을 클릭해서 서비스를 활성화합니다.
    <br><br>
* 활성화 확인
    ![](http://static.toastoven.net/prod_autocomplete/product-use-03-20180724.png)
    1. "Search" 클릭합니다.
    2. "Autocomplete"가 노출되면 활성화된 것입니다.

## 기본 사용법

### 서비스 생성
* 서비스 생성 방법
    ![](http://static.toastoven.net/prod_autocomplete/domain_create_procedure.png?)
    1. "서비스 생성" 버튼을 클릭합니다.
    2. 서비스 ID를 입력합니다.
        * 영문 소문자, 숫자 및 일부 특수 문자만 사용 가능합니다.
        * 사용 가능한 특수 문자
        ```
        '~' '@' '$' '&' '(' ')' ':' '_' '-'
        ```        
    3. "저장" 버튼을 클릭합니다.
    <br><br>
* 서비스 생성 결과
    ![](http://static.toastoven.net/prod_autocomplete/domain_create_result.png?)
    1. 생성된 서비스 ID(test)를 클릭합니다.

### 색인
* 색인할 파일 생성
    * 아래 예제와 같은 형식으로 색인 요청 파일을 생성합니다.
    * <span style="color:red">색인할 파일은 UTF-8로 생성해야 합니다.</span>
        * Windows 메모장에서 파일 저장시 인코딩을 UTF-8로 지정해서 저장합니다.
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
    * 최대 파일 사이즈는 32MB입니다.
    <br><br>
* 색인 방법
    ![](http://static.toastoven.net/prod_autocomplete/indexing_procedure_01.png)
    ![](http://static.toastoven.net/prod_autocomplete/basic-indexing-20180724.png)
    1. "색인" 탭을 클릭합니다.
    2. "파일 선택" 버튼을 클릭합니다.
    3. 색인할 파일을 선택합니다.
    4. "열기" 버튼을 클릭합니다.  
    5. 색인 명령어가 Rest API로 출력됩니다.
    6. "색인" 버튼을 클릭합니다.
    7. "새로 고침" 버튼을 클릭합니다.
    8. 색인 결과를 확인합니다.
    <br><br>
* 색인 주의 사항
    * <span style="color:red">색인을 요청하면 기존 데이터는 모두 삭제되고 신규 데이터로 교체됩니다.</span>
    <br><br>
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
            * id 1은 위의 색인 API Response 의 id입니다.
        * Response
            ```
            {
              "request_time" : "2017-10-23T12:36:43",
              "file_name" : "documents.json",
              "file_size" : 114,
              "status" : 4
            }
            ```
            * status
                * 1 : 대기 중
                * 2 : 무시됨
                * 3 : 진행 중
                * 4 : 성공
                * 5 : 실패

### 자동완성
* 자동완성 방법
    ![](http://static.toastoven.net/prod_autocomplete/autocomplete_procedure.png??)
    1. "자동완성" 탭을 클릭합니다.
    2. 검색할 단어를 입력합니다.
    3. 출력 개수를 지정합니다.
    4. 자동완성 Rest API입니다.
    5. 자동완성 결과가 출력됩니다.
    <br><br>
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
* 색인 및 자동완성 REST API를 호출할 수 있는 장비의 IP를 제한할 수 있습니다.
    * 콘솔에서 테스트하는 경우 ACL 설정과 관련 없습니다.
* ACL 설정 방법
    ![](http://static.toastoven.net/prod_autocomplete/acl_procedure.png???)
    1. "ACL" 탭을 클릭합니다.
    2. 색인 요청 IP 주소가 202.179.177.21 인 경우만 색인이 가능하도록 설정한 예제입니다.
    3. 자동완성 요청은 모든 IP에서 가능하도록 설정한 예제입니다.
    4. "저장" 버튼을 클릭합니다.  

## 기능 가이드

### 부가 정보 출력
* 색인
    * 테스트를 위해 아래 데이터를 색인합니다.
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
    * payload에 출력하고 싶은 부가 정보를 입력합니다.
    <br><br>
* 자동완성
    ![](http://static.toastoven.net/prod_autocomplete/suggest-extra_info.png?)
    1. 이미지 URL과 카테고리 정보가 출력됩니다.

### Input/Output을 다르게 설정
* 색인
    * 테스트를 위해 아래 데이터를 색인합니다.
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
    <br>
* 자동완성
    ![](http://static.toastoven.net/prod_autocomplete/input-output.png?)
    1. "나"를 입력했을 때
    2. "Nike"가 출력됩니다.

### 중간 매칭
* 색인
    * 테스트를 위해 아래 데이터를 색인합니다.
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
    <br>
    * 색인할 때 "중간 매칭"을 체크합니다.
    ![](http://static.toastoven.net/prod_autocomplete/infix-indexing-20180724.png)
    <br>
* 자동완성
    ![](http://static.toastoven.net/prod_autocomplete/infix-suggest-20180724.png)
    1. "운동"을 입력했을 때
    2. 중간에 "운동"으로 시작되는 "나이키운동화"가 출력 됩니다.

### 한영타 변화
* 색인
    * 테스트를 위해 아래 데이터를 색인합니다.
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
    <br>
    * 색인할 때 "한영타 변환"을 체크합니다.
    ![](http://static.toastoven.net/prod_autocomplete/koreng-indexing-20180724.png)
    <br>
* 자동완성
    ![](http://static.toastoven.net/prod_autocomplete/suggest-haneng.png?)
    1. "나이"의 영타인 ""skdl"를 입력했을 때
    2. "나이키"가 출력 됩니다.

### 초성 자동완성
* 색인
    * 테스트를 위해 아래 데이터를 색인합니다.
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
    <br>
    * 색인할 때 "초성 자동완성"을 체크합니다.
    ![](http://static.toastoven.net/prod_autocomplete/chosung-indexing-20180724.png)
    <br>
* 자동완성
    ![](http://static.toastoven.net/prod_autocomplete/suggest-cho.png???)
    1. "ㄴㅇㅋ"를 입력했을 때
    2. "나이키"가 출력 됩니다.

### Multi 서비스
* 2개 이상 서비스의 자동완성 결과를 한 번의 자동완성 API 요청으로 출력되도록 하는 기능입니다.
    * 예를 들어 브랜드와 카테고리 자동완성을 한 번의 API 요청으로 출력할 때 사용합니다.
* brand 서비스 생성
    ![](http://static.toastoven.net/prod_autocomplete/domain_create-brand.png)
    <br>
* brand 색인
    * 테스트를 위해 아래 데이터를 색인합니다.
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
    <br>
* category 서비스 생성
    ![](http://static.toastoven.net/prod_autocomplete/domain_create-category.png)
    <br>
* category 색인
    * 테스트를 위해 아래 데이터를 색인합니다.
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
    <br>
* 자동완성
    ```
    [admin@NHNEnt:~]$ curl -G -XGET 'http://alpha-api-autocomplete.cloud.toast.com/autocomplete/v1.0/appkeys/3PrEhyNmfipIHMkZ/serviceids/brand,category/autocomplete?count=10' --data-urlencode query='나'
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
    * API 호출시 "serviceids/brand,category"로 요청했습니다.
    * API 응답에 index 0 는 brand, index 1 은 category 자동완성 결과가 출력 됩니다.

## 상세 가이드

### 출력 우선 순위
* 색인 파일이 아래와 같을 경우 사용자가 'ㄴ' 을 입력하면 "노트북", "나이키", "남성상의" 순으로 출력됩니다.
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
    * 'ㄴ'으로 시작하는 단어 중 weight 가 높은 순으로 출력됩니다.
### ACL
* ACL 설정 화면
    ![](http://static.toastoven.net/prod_autocomplete/detail-acl.png?)
* 입력형식
    * IP 형식으로 입력 가능합니다.
        * 예제) 202.179.177.21
    * CIDR 형식으로 입력 가능합니다.
        * 예제) 202.179.177.0/24
    * IP 또는 CIDR 을 여러 개 입력 가능합니다.
        * 예제) 202.179.177.21, 202.179.177.0/24
    * all 일 경우 모두 매칭 됩니다.
    * 값이 비어 있을 경우 모두 매칭 안됩니다.  
* 허용, 거부 둘 다에 매칭이 될 경우 거부됩니다.
* 허용, 거부 둘 다에 매칭이 안될 경우 거부됩니다.  
