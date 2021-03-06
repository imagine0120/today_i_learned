# 효율적인 서버 관리를 찾아서, 서버리스(Serverless)
* https://blog.ncsoft.com/platform-center-03-20210224/?utm_source=gaerae.com&utm_campaign=%EA%B0%9C%EB%B0%9C%EC%9E%90%EC%8A%A4%EB%9F%BD%EB%8B%A4&utm_medium=social

### 서버리스(Serverless)의 개념
* 서버가 없는 것처럼, 서버를 고려하지 않고 애플리케이션을 실행할 수 있는 환경
* 클라우드 기반 인프라를 이용하여 **자원을 효율적으로 사용하고 비용을 절감**
* 클라우드와 달리 'on-demand', 즉 요청이 있을 때만 구동됨
* FaaS(Function as a Service) + BaaS(Backend as a Service) 의 개념
* BaaS : Firebase나 Auth0 같이 특정 기능을 API 형태로 사용할 수 있도록 제공하는 서비스

### Knative
* Google Cloud Service에서 IBM, Red Hat, SAP 와 협력해 개발한 오픈소스 서버리스 프레임워크 (k8s + Istio)
* 주요 기능
   * Auto Scaling을 통한 리소스 절감 : 일정 시간 호출되지 않으면 pod을 자동 종료하고, 서비스 요청이 많아지면 pod 개수를 설정한 최댓값까지 확장
   * 카나리 배포를 통한 리스크 감소 : 간단한 설정으로 일부 트래픽만 새로운 버전으로 분산시켜 서비스가 정상 동작하는지 확인 가능
   * 이벤트 트리거 : 이벤트 발생 시 처리 및 전달해주는 비동기 매커니즘 제공. Kafka, pub/sub, cron 등의 자원을 등록하고 이벤트 발생 시 지정된 Knative 서비스로 메시지 전송 가능
   * 편리한 모니터링 : Kiali는 서비스 간 트래픽 라우팅에 대한 직관적 시각자료 제공

### 서버리스 도입 시 고려할 사항
* 서비스 지연 가능성 : 'always-on'이 아닌, 'on-demand'이기에 요청 시마다 인스턴스 구동하고 애플리케이션 실행하는 데에 시간이 소요됨
* 최적화 이미지 필요 : 단순히 jar 파일을 도커 이미지에 복사하면 빌드할 때마다 jar 크기만큼의 도커 레이어가 생성됨. 의존성 라이브러리가 포함된 'fat jar'의 압축을 풀어, 의존성과 애플리케이션 코드 부분의 레이어를 분리하면 빌드 및 배포 시간을 줄일 수 있음. Google에서 제공하는 Jib을 통해 최적화된 도커 이미지 생성 가능
* HotSpotVM 대신 GraalVM : 기존 OpenJDK의 HotSpotVM은 준비시간이 필요해서 지연 생김. 그러나 GraalVM은 MS에 이상적인 고성능 런타임 JVM. GraalVM에서 제공하는 Native Image를 이용하면 startup 시간이 최대 100배 빨라지며, 메모리 사용량은 최대 5배 줄일 수 있음. 그러나 Native Image 사용하여 컴파일 시 런타임에서 동적 클래스 로딩\*이 불가능하다는 단점이 있음.
   * 동적 클래스 로딩(Reflection) : 구체적인 클래스를 몰라도 해당 클래스를 호출하고 메소드, 변수 등에 접근 가능하게 해주는 
* 스프링부트의 대안 프레임워크 : Quarkus, Micronaut, Helidon 등의 Java Microservice 프레임워크는 스프링에 비해 startup 시간 및 메모리 사용량이 적음.

<hr/>

### 읽고 나서
* 아직은 도커와 쿠버네티스, 클라우드 환경에 대한 이해도가 적어 완전히 이해하지는 못했다. 하지만 상시 서비스를 제공하는 것이 아니라, 요청에 따라 효율적으로 자원을 이용하며 서비스를 제공하는 서버리스의 개념은 확실히 매력적이다. 언젠가는 도커, 쿠버네티스는 물론, Knative와 같은 서버리스 프레임워크도 사용하여 서비스를 제공해보고 싶다.

### 더 찾아볼 것
* Kafka, pub/sub : 대용량 트래픽 처리와 Kafka가 함께 언급되는 것을 몇번 본 것 같은데, 정확히 Kafka가 무엇인지 모르겠다. 
* HotSpotVM과 GraalVM : 런타임 JVM의 개념 알아보기. 지금 설명해보라고 한다면, '자바로 짠 프로그램이 OS 독립적으로 실행될 수 있게 하는 가상의 환경'으로 대답할 텐데, 조금 더 공부해야겠다.
* 런타임 시 동적 클래스 로딩 : JNI, Java Reflection 등이 동적 기능이라고 언급되었는데, 어떤 개념인지 감이 안 잡힌다.
