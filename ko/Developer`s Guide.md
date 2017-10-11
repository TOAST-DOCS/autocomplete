## Upcomming Products > Autocomplete > Developer's guide

## 상세 설명

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
