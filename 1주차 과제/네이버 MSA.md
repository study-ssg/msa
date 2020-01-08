[네이버 동영상 플랫폼 MSA]

https://d2.naver.com/helloworld/3963776

MSA 도입 전 2가지 주요 고민
- MSA 구성으로 인한 시스템 전체의 복잡도를 어떻게 낮출 수 있을까?
- 기존 서비스에 영향을 미치지 않고 어떻게 안정적으로 MSA 구성으로 전환할 수 있을까?

도입 서비스
Eureka: 서비스 디스커버리의 구현체
Zuul: API 게이트웨이의 구현체
Hystrix: 서킷 브레이커의 구현체
Spring Cloud Config / Spring Cloud Seluth/ Zipkin

1. Eureka

내부 애플리케이션이 서로 통신하기 위한 서비스
기존 L4 방식으로는 수많은 모듈의 로드밸런싱 어려움
서비스 디스커버리는 매번 네트워크 주소가 변경될 수 있는 분산 환경에서 노드 사이의 통신을 위해 네트워크 주소를 찾는 패턴
Ribbon/Feign/Eureka Client 3가지 방식 존재

	1. Application Service가 Eureka Server에 자신의 네트워크 정보 저장
	2. Application Client가 Eureka Server로부터 Registry 정보 수신

	* Eureka 설정값에 따른 내부 동작 이해
	Eureka 인스턴스 - leaseRenewalIntervalInSeconds(Eureka 서버에 자신의 상태 제공 주기), leaseExpirationDurationInSeconds(renew 실패 시 Eureka 인스턴스 제외 Timeout 수치)
	Eureka 클라이언트 - registryFetchIntervalSeconds(Registry 로컬 캐시 저장 주기), instanceInfoReplicationIntervalSeconds(Registry 변경사항 복제 주기)

	* 장애 패턴에서 주의할 점
	enableSelfPreservation 설정을 통해 클라이언트가 대규모 누락되는 사태 방지

	* Eureka 사용 시 배포를 위한 트래픽 제어
	기본 설정값 기준 down후 실제 트래픽 차단까지 최대 90초가 걸림. 헬스체크는 필수 (healthcheck.enabled)

2. Zuul

Spring Cloud의 API 게이트웨이 구현체
모든 클라이언트에서 로드 밸런싱을 구현하기는 불가능에 가까우므로, 외부 요청을 라우팅 할 API 게이트웨이가 필요
백엔드에 있는 서비스 애플리케이션은 주기적으로 Eureka에 Registry 정보를 등록하고, Zuul은 이 정보를 바탕으로 외부의 요청을 내부 서비스로 라우팅
필터를 통해 요청에 대한 인증을 처리하고 요청에 대한 지표나 데이터를 수집한다.
API 게이트웨이를 구성 컴포넌트와 역할

3. Hystrix

백엔드에 있는 일부 애플리케이션에 장애가 발생하거나 응답 속도가 늦어지면 스레드 풀이 고갈되어 전체 장애로 이어짐

	* 특징
		1) 빠른 실패 처리와 복구(fail fast & rapidly recover)
			요청이 너무 많거나 문제가 발생해 더 이상 요청을 처리할 수 없다면 빠르게 실패로 처리한다.
			장애가 지속되는 것을 방지하기 위해 문제가 발생한 곳으로 트래픽을 더 이상 보내지 않는다.
		2) 실패 대체(fallback)
			가능하다면 실패를 대체할 수 있는 처리를 해 응답한다.
		3) 격리 처리(bulkhead)
			하나에서 문제가 발생해도 나머지는 정상으로 작동하도록 스레드 풀을 두어 격리한다.
		4) 서킷 브레이커
			실패율을 모니터링해 문제가 발생하면 처리 요청을 차단한다. 정상으로 회복되면 다시 요청을 처리한다.
			서킷이 Closed 상태이면 문제가 없는 상태다. 서킷의 초기 상태다.
			서킷이 Open 상태이면 실패율이 임곗값을 넘어간 상태다. 더 이상 요청을 처리하지 않고 빠르게 실패로 처리한다.
			서킷이 Half-Open 상태이면 Open 상태에서 일정 시간이 지난 상태다. 요청을 시도해 성공하면 Closed 상태로, 실패하면 Open 상태로 돌아간다.
			
