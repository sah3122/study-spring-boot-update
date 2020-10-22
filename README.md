# 인프런 스프링 부트 업데이트 강의 정리

## 스프링 부트 2.1 주요 변경 내역 
* 자바 11 지원
* 스프링 데이터 JPA, lazy 모드 지원
* 의존성 변경
* 빈 오버라이딩을 기본으로 허용하지 않도록 변경
* Acutator에 “/info”와 “/health” 공개하도록 바뀜
* 프로퍼티 변경
* 로깅 그룹
    * Spring-JCL
        * Logging Facade 와 Logger 
            * Logging Facade 는 Logger 를 선택해서 사용하는 방식이다. 
            * 스프링 프레임워크는 로깅 퍼사드를 사용한다.
            * 실제 로거 구현체는 개발자가 선택해서 사용할 수 있는 구조이다.
            * 현재는 slf4j를 사용하며 컴파일 타임에서 로거를 선택하게 된다. 
            * 로거는 보통 logback 과 log4j2 중에 하나를 사용하게 된다.

        * 기존에 이미 다른 로깅퍼사드나 로거를 사용중인 경우 어떻게 해결할지 ? 
            * slf4j로 통하는 bridge를 놓는다. (Jcl-over-slfj4, log4j-to-slf4j)
        * slf4j가 사용할 로거는 어떻게 정할까 ? (여러개의 로거가 등록되어 있을때) 
            * 사용할 로거를 정해준다 (Binder)
            * Logback-classic, log4j-slf4j
        * Spring-jcl의 동장
            * JCL-over-SLF4J 대체제
            * 클래스 패스에 Log4j 2 가 있다면 JCL을 사용한 코드가 Log4j 2를 사용
            * 클래스 패스에 SLF4j가 있다면 JCL을 사용한 코드가 SLF4J를 사용한다. 
        * 스프링 부트 로깅 시스템
            * SLF4J, logback-classic -> Logback
            * SLF4J에서 logback-classic 을 통해서 최종적으로 Logback을 사용하는 구조이다.
* 빈 오버라이딩 기본 정책 변경
    * 스프링 부트는 빈을 두 과정에 거쳐 등록하게 된다. 
        1. 애플리케이션에서 정의한 빈 등록
        2. 자동설정(AutoConfiguration)이 제공하는 빈 등록. 
            * spring.factories 파일을 통해 읽어 들임
        * 이때 1번에서 정의한 빈을 2번 과정에 등록하는 빈이 재정의(overriding) 할 수도 있는데,  
          스프링 부트 2.1 이전까지는 그런 기능을 기본으로 허용했지만 2.1 이후부터는 허용하지 않는다.
    * 프로퍼티를 변경해서 빈 오버라이딩을 허용할 수도 있다.
        * spring.main.allow-bean-definition-overriding=true
    * 프로퍼티에서 빈 오버라이딩을 허용하게 되면 우리가 등록한 빈이 아닌 자동설정에서 등록한 빈이 우선순위를 가지게 되어 권장하는 방법이 아니다.
    * 오버라이딩이 일어나지 않도록, 자동 설정을 제공하는 쪽에 @Condition*(ConditionalOnMissingBean) 애노테이션을 활용할 것

         
