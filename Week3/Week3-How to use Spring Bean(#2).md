# [백엔드 스터디 in 큐시즘 - D조] Week 3: 스프링의 전반적인 이해
## #2. 스프링에서 빈 객체를 관리하는 방법은 무엇인가

### Spring IoC 컨테이너에 빈을 등록하는 방법

1) Component Scan 
2) Java 코드로 등록

### 1) Component Scan
컴포넌트 스캔은 @Component를 명시하여 빈을 추가하는 방법이다. 클래스 위에 @Component를 붙이면 스프링이 알아서 스프링 컨테이너에 빈을 등록한다. (@Component를 포함하는 @Controller, @Service, @Repository 애노테이션도 스프링 빈으로 자동 등록된다.)


``` java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Indexed
public @interface Component {
```

### 2) Java 코드로 등록
Java 코드로 빈을 등록할 수 있다. 클래스를 생성하고, 위에서 언급한 @Configuration 어노테이션을 활용한다. 
``` java
@Configuration // @Configuration을 이용하면 @Component를 포함하는 @Controller, @Service, @Repository 애노테이션도 스프링 빈으로 자동 등록된다.
public class AppConfig {

    @Bean //빈 등록하기 위해 인스턴스 생성하는 메소드 위에 @Bean 명시
    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }

    @Bean
    public MemberService memberService() {
        return new MemberServiceImpl(memberRepository());
    }

}
```
### 3) @Bean vs @Component
@Bean
- 개발자가 컨트롤이 불가능한 외부 라이브러리들을 Bean으로 등록하고 싶은 경우
에 사용된다.
- 메소드 또는 어노테이션 단위에 붙일 수 있다.
- 반환되는 객체(인스턴스)를 개발자가 수동으로 빈으로 등록하는 애노테이션
  
@Component
- 개발자가 직접 컨트롤이 가능한 클래스들의 경우에 사용된다.
- 클래스 또는 인터페이스 단위에 붙일 수 있다.
- 스프링이 런타임시에 컴포넌트스캔을 하여 자동으로 빈을 찾고(detect) 등록하는 애노테이션
  
### 4) 정리
일반적인 빈 등록은 간편하게 @Component 어노테이션으로, 유연한 빈 등록이 필요하다면 @Configuration 어노테이션이 들어간 클래스 내 @Bean 어노테이션 메소드 선언으로!
참고로, Spring Boot의 경우 @SpringBootApplication 어노테이션이 들어간 스프링 실행부에서도 @Bean 어노테이션이 깃든 메소드 등록이 가능하다.


참고한 블로그
<br>
[Spring Bean 총정리](https://steady-coding.tistory.com/594)
<br>
[스프링 빈(bean)이란? 스프링 빈 등록하는 방법](https://velog.io/@falling_star3/Spring-Boot-%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B9%88bean%EA%B3%BC-%EC%9D%98%EC%A1%B4%EA%B4%80%EA%B3%84)
<br>
[스프링 빈을 등록하는 두 가지 방법](https://mimah.tistory.com/entry/Spring-%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B9%88%EC%9D%84-%EB%93%B1%EB%A1%9D%ED%95%98%EB%8A%94-%EB%91%90-%EA%B0%80%EC%A7%80-%EB%B0%A9%EB%B2%95)
<br>
[스프링 빈을 등록하는 두 가지 방법](https://mimah.tistory.com/entry/Spring-%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B9%88%EC%9D%84-%EB%93%B1%EB%A1%9D%ED%95%98%EB%8A%94-%EB%91%90-%EA%B0%80%EC%A7%80-%EB%B0%A9%EB%B2%95)
