# 우아한형제들 MSA 사례

* http://woowabros.github.io/r&d/2017/06/13/apigateway.html

* 기존 LEGACY시스템을 MAS구조로 전환하는데 개발비용과 운영상 이슈 존재
* API Gateway를 도입하여 기존 LEGACY시스템을 신규서비스로부터 고립

* MSA 환경에서는 수많은 서비스들이 생겨나기 때문에 client와의 endpoint도 증가하여 관리가 힘듬
* endpoint들을 통합하기 위해 API Gateway 필요

* Netflix Zuul 선택
  * 이유 : JAVA프로젝트, Netflix는 세계에서 MSA를 가장 잘하고 있는 회사
  
* Zuul은 zuul-core, zuul-simple-webapp, zuul-netflix, zuul-netflix-webapp 4개의 컨포넌트로 구성
  * zuul-core : 위에서 설명한 Zuul의 Request Lifecycle를 담당하고, Fliter를 컴파일하고 실행한는 기능을 담당하고 있는 Zuul의 core library
  * zuul-netflix : 기본적은 Zuul에 NetflixOSS library를 추가한다.
  * zuul-simple-webapp : zuul-core만 사용한 아주 기본적은 web application
  * zuul-netflix-webapp : zuul-core와 zuul-netflix를 함께 사용한 web application
  
* zuul-netflix-webapp을 도입하기에는 학습곡선이 커서, spring cloud netflix로 전환

* 배달의민족 LEGACY

![legacy](https://user-images.githubusercontent.com/13361971/71976744-69b2ea80-325a-11ea-8880-471aa66c3d63.png)


* 배달의민족 MSA

![new-api](https://user-images.githubusercontent.com/13361971/71976772-7d5e5100-325a-11ea-8b30-fc513c30baa8.png)

* 기존 API 서버에 사용했던 ELB에 API GATEWAY를 구성했고
  두 개의 ELB를 더 생성하여, 하나의 ELB에는 기존 LEGACY API 서버를 구성하였고, 또 하나의 ELB에는 새로운 API 서버로 구성
