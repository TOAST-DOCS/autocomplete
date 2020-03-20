## Search > Autocomplete > 개요

- 검색 창에 검색어를 입력할 때 자동 완성 기능을 제공하는 서비스입니다.
    - 색인 REST API를 이용해서 자동 완성에 사용할 데이터를 입력합니다.
    - 자동 완성 REST API를 이용해서 자동 완성 결과를 받아옵니다.

### 자동 완성 서비스 개발 과정

서비스 구성도는 다음과 같습니다.

![img](http://static.toastoven.net/prod_autocomplete/block_diagrm-ko-20200304.png)

서비스 개발 과정

1. 서비스 생성

    - 자동 완성 서비스를 생성합니다.

2. 색인

    - Autocomplete의 입력 형식에 맞게 JSON 데이터를 생성합니다.

    - 생성된 JSON 데이터를 REST API를 이용해서 Autocomplete에 입력합니다.

3. 자동 완성

    - 자동 완성 REST API의 결과를 이용해서 프런트 화면을 구성합니다.
