# 쿠팡 MSA 
https://medium.com/coupang-tech/%ED%96%89%EB%B3%B5%EC%9D%84-%EC%B0%BE%EA%B8%B0-%EC%9C%84%ED%95%9C-%EC%9A%B0%EB%A6%AC%EC%9D%98-%EC%97%AC%EC%A0%95-94678fe9eb61

## Legacy Pain Point (레거시 구조는 현재 우리 구조랑 비슷해 보였음)
1. 부분의 장애가 전체의 서비스 장애로 확대되는 문제가 있다.
- 배송의 장애가 주문의 장애로까지 이어져서는 안된다.
2. Monolithic architecture는 부분적인 Scale-out을 하기 어렵다.
 - 필요한 부분의 Scale-out을 위해 전체를 Scale-out해야한는 상황
3. 여러 컴포넌트가 하나의 서비스에 강결합 형태로 되어 있어 서비스의 변경이 매우 어렵고, 수정시 장애 영향도를 파악하기 힘들다.
 - 모여라 꿈동산 소스코드 (ex. pd-lib)
4. 작은 변경에도 높은 수준의 테스트 비용이 발생한다.
 - 주문을 수정했는데 배송 데이터 생성까지 테스트 해야하는 상황 (연결되어있다면)
5. Monolithic architecture에서 조직(개발자/팀)이 성장할수록, 배포의 대기 시간이 비약적으로 증가한다.
 - ex) pd-lib의 간단한 수정이지만 얽힌 코드가 많아 정기배포를 이용해야함.

## 쿠팡의 MSA 전략
1. Vitamin Framework의 개발 
- micro service architecture를 위한 Java 기반의 framework
- micro service architecture를 위한 표준 skeleton code template을 포함.
- 표준 skeleton code template에는 작은 도메인 단위의 front / api / batch / back-office 서비스를 쉽게 개발하기 위한 기본 코드들이 포함되어 있으며, 쿠팡에서 개발한 다양한 표준 라이브러리들도 쉽게 사용할 수 있도록 되어 있음.
- 도메인팀에서 별다른 노력없이 테스트, 배포, 모니터링, 자동 복구가 가능하며, Message Queue, Cache 등 모든 platform 서비스와 쉽게 연동
2. Provider helper library (api-adapter)
- 특정 API를 사용하기 위하여 모든 client들은 HTTP 통신하는 모듈을 만들고, json 형태의 API를 호출한 뒤 Object로 매핑하는 로직을 구현하여야 한다.
- 쿠팡은 Provider helper library (api-adapter)를 같이 제공하여 API를 쉽게 사용할 수 있는 전략을 취하였다.
3. Message Queue를 이용한 Transaction 의 분리
- 주문 데이터 생성 후 카프카로 message 전송 배송은 카프카에서 데이터 polling 하여 배송 데이터 생성

## 쿠팡의 MSA 플랫폼
https://medium.com/coupang-tech/%ED%96%89%EB%B3%B5%EC%9D%84-%EC%B0%BE%EA%B8%B0-%EC%9C%84%ED%95%9C-%EC%9A%B0%EB%A6%AC%EC%9D%98-%EC%97%AC%EC%A0%95-a31fc2d5a572

1. Configuration Management Database (CMDB)
- 쿠팡의 CMDB 시스템은 서비스와 서비스를 구성하는 모든 자원들을 위한 메타 DB이며, Key/Value Collection 으로 구성된 metadata가 상호간 관계를 가지는 구조.
- 지금 우리가 구성하는 Rancher 같은 역활인듯?
2. Coupang Deployment System
- blue / green deployment strategy를 가진 cloud 기반의 웹 배포 시스템
- 10초 이내 롤백을 지원하여 서비스 장애가 발생하더라도 빠르게 복구가 가능
3. AB Test
- A안 / B안을 전체 또는 일부 고객에게 노출하여 검증하는 시스템을 AB Test라고 부른다
- 특정 A/B에 대하여 디바이스(아이폰, 안드로이드, PC)를 선택하고 노출빈도, 기간을 설정하면 자동으로 A/B 테스트가 진행된다. 이 테스트를 통하여 Gross Merchandise Volume, Conversion Rate 등의 지표가 어떻게 변화하는지 판단한다
4. Coupang API Gateway
- API Gateway를 자체적으로 구축
5. Confidence System
- 쿠팡의 Production 환경을 위한 배포 파이프라인은 Stage > Canary > All 단계로 구성
- Stage : 사용자의 트래픽이 들어오지 않은 상태에서 테스트하는 Stage 단계
- Canary : 신규 feature를 1대만 배포하여 사용자 트래픽을 제한적으로 받아 테스트 하는 Canary 단계
- All : 신규 feature를 모든 서버에 배포하는 단계
6. Circuit Breaker System
 - 쿠팡은 Valve 라고 불리는 자체적인 Circuit Breaker System을 개발 운영
 - 특정 서버의 장애가 발생하면, 자동으로 해당 서버를 제거하고 추가 서버를 자동으로 투입함으로써 특정 서버의 장애가 서비스의 장애로 확대하지 않도록 한다. 또한 장바구니 서비스의 장애가 발생하면, 상품 페이지에서 장바구니 버튼을 숨겨서 고객이 바로 주문으로 넘어가도록 유도해 장애를 회피할 수 있다
