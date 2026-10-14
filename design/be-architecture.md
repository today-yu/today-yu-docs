# Backend Architecture

<img width="1431" height="582" alt="image" src="https://github.com/user-attachments/assets/a985b7b7-72cc-4d60-bd41-de313ff28728" />

<br/>

&nbsp; 본 프로젝트는 도메인 주도 설계(DDD, Domain Driven Design) 기반으로 구성된다. 

&nbsp; 또한, 각 계층(layer)를 하위 모듈로 구성한 멀티 모듈 프로젝트로 컴파일 시점에서 의존 전이를 제한하여, 외부 → 내부로 의존성을 제한하여 핵심 비즈니스 로직과 도메인 엔티티의 의미를 강조한다. 

&nbsp; 헥사고날 아키텍처(Hexagonal Architecture: Ports and Adapter)를 사용하여 외부 인프라·기술에 의한 핵심 비즈니스 로직에 변경 가능성과 결합도를 낮춘다.

## 1. 하위 모듈 설명

&nbsp; 각 모듈에 대한 구체적인 책임은 아래와 같다.

### 1-1) domain

&nbsp; `domain` 모듈은 도메인 객체(엔티티)와 관련 객체를 정의한 모듈로, 외부 프레임워크 의존성이 없는 순수 Java 코드로 작성된다.

> &nbsp; 단, 개발 편의성을 위하여 Lombok을 사용하되 데이터 변경 위험이 존재하는 `@Setter`, `@Data` 등과 같은 어노테이션은 `lombok.config` 설정 파일을 통하여 컴파일 시점에 차단한다.

### 1-2) application

&nbsp; `application` 모듈은 서비스의 핵심 비즈니스 로직(Usecase)을 구현한다. 또한, 유즈케이스에서 사용하는 각종 Port 인터페이스와 DTO를 정의하는 코어(Core) 모듈이다.

### 1-3) persistence

&nbsp; `persistence` 모듈은 `application` 모듈의 Output Port 인터페이스 중 데이터베이스와 실제 통신을 담당하는 기능을 구현하는 어댑터 모듈이다.

### 1-4) infrastructure

&nbsp; `infrastructure` 모듈은 `application` 모듈의 Output Port 인터페이스 중 데이터베이스를 제외한 모든 외부 시스템과의 통신을 담당하는 기능을 구현하는 어댑터 모듈이다.

### 1-5) apis

&nbsp; `apis` 모듈은 사용자로부터 요청을 받는 모듈로 Controller 및 요청·응답에 사용하는 DTO를 구현한 모듈이다.

&nbsp; 사용자와 통신을 담당하는 `apis-user`, 관리자와 통신을 담당하는 `apis-admin` 하위 모듈로 나뉘며, 두 모듈이 공통으로 사용하는 기능 또는 DTO는 `apis-common` 모듈에 구현한다.

### 1-6) boot

&nbsp; `boot` 모듈은 애플리케이션 구동을 위한 시작점을 담당하며, 모든 모듈을 하나로 통합하고 실행하는 모듈이다.

## 2. 의존성 규칙 

1. 모든 의존성은 외부 → 내부로 향해야 한다.
2. `domain` 모듈은 어떠한 계층도 의존하지 않는다.
3. `application` 모듈은 `domain` 모듈을 의존한다.
4. `application` 모듈을 제외한 다른 모듈을 필요에 의해 선택적으로 `domain` 모듈을 의존한다.
