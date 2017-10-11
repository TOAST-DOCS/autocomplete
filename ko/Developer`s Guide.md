## Upcomming Products > Autocomplete > Developer's guide

## 기본 사용법

### 도메인 생성
* 도메인 생성 방법
    ![](http://static.toastoven.net/prod_autocomplete/domain_create_procedure_01.png?)
    1. "도메인 생성" 버튼을 클릭합니다.
    2. 도메인 이름을 입력합니다.
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

### 자동완성
* 자동완성 방법
    ![](http://static.toastoven.net/prod_autocomplete/autocomplete_procedure.png?)
    1. "자동완성" 탭을 클릭합니다.
    2. 검색할 단어를 입력합니다.
    3. 자동완성 Rest API 입니다.
    4. 자동완성 결과가 출력됩니다.   

### ACL
* ACL 설정 방법
    ![](http://static.toastoven.net/prod_autocomplete/acl_procedure.png???)
    1. "ACL" 탭을 클릭합니다.
    2. 색인 요청 IP 주소가 202.179.177.21 인 경우만 색인이 가능하도록 설정한 예제입니다.
    3. 자동완성 요청은 모든 IP 에서 가능하도록 설정한 예제입니다.
    4. "저장" 버튼을 클릭합니다.  
<br>
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
