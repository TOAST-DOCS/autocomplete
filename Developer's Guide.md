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
  ``` json
  ```
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
![](http://static.toastoven.net/prod_search/acl_procedure.png)
  1. "ACL" 탭을 클릭합니다.
  2. "관리" 는 현재 사용하지 않습니다.
  3. 색인 요청 IP 주소가 133.186.133.112 인 경우만 색인이 가능하도록 설정한 예제입니다.
     * 133.186.133.112, 133.186.133.113 형식으로 IP 를 여러개 지정 가능합니다.
     * 133.186.133.0/24 와 같이 CIDR 형식으로 지정 가능합니다.
  4. 검색 요청은 모든 IP 에서 가능하도록 설정한 예제입니다.
  5. "저장" 버튼을 클릭합니다.  

  
