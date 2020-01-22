## Search > Autocomplete > 개요

* 검색창에 질의를 입력할때 자동완성 기능을 제공하는 서비스입니다.
    * 색인 REST API를 이용해서 자동완성에 사용할 데이터를 입력합니다.
    * 자동완성 REST API를 이용해서 자동완성 결과를 받아옵니다.

### 자동완성 서비스 개발 과정
* 서비스 구성도
![](http://static.toastoven.net/prod_autocomplete/block_diagrm-20200113.png)

* 서비스 개발 과정
    1. 서비스 생성
        * 자동완성 서비스를 생성합니다.
    2. 색인
        * Autocomplete의 입력 형식에 맞게 JSON 데이터를 생성합니다.
        * 생성된 JSON 데이터를 REST API를 이용해서 Autocomplete에 입력합니다.
    3. 자동완성
        * 자동완성 REST API의 결과를 이용해서 Front 화면을 구성합니다.
