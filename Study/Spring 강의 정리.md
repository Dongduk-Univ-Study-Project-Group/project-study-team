# Inversion of Control

제어권의 역전

### 일반적인 (의존성에 대한) 제어권:

```java
class OwnerController {
	private OwnerRepository repository = new OwnerRepository();
}
```

### IoC: 외부에서 의존성 주입

```java
class OwnerController {
	private OwnerRepository repo;
	public OwnerController(OwnerRepository repo) {
		this.repo = repo;
	}
	// repo를 사용한다.
}

class OwnerControllerTest {
	@Test
	public void create() {
		OwnerRepository repo = new OwnerRepository();
		OwnerController controller = new OwnerController(repo);
	}
}
```

참고

- [https://martinfowler.com/articles/injection.html](https://martinfowler.com/articles/injection.html)

# IoC (Inversion of Control) 컨테이너

ApplicationContext (BeanFactory)

### 빈(bean)을 만들고 엮어주며 제공해준다.

녹색 콩으로 보여진다.

@Bean 으로 만들 수 있다.

### 빈 설정

- 이름 또는 ID
- 타입
- 스코프

### 참고

- [https://github.com/spring-guides/understanding/tree/master/application-context](https://github.com/spring-guides/understanding/tree/master/application-context)
- [https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/ApplicationContext.html](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/ApplicationContext.html)
- [https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/BeanFactory.html](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/BeanFactory.html)

# 빈 (Bean)

스프링 IoC 컨테이너가 관리하는 객체

### 등록방법

- Component Scanning
    - @Component  **lifecycle callback**
        - @Repository
        - @Service
        - @Controller
        - @Configuration
- 또는 직접 일일히 XML이나 자바 설정 파일에 등록

### 사용방법

- @Autowired 또는 @Inject
- 또는 ApplicationContext에서 getBean()으로 직접 꺼내거나

```java
@Autowired
ApplicationContext applicatonContext;

@Test
public void getBean() {
	applicationContext.getBeanDefinitionNames();
	getBean(s:””);
}
```

### 특징

- 의존성 주입은 빈끼리만 가능

# 의존성 주입 (Dependency Injection)

필요한 의존성을 어떻게 받아올 것인가..

### @Autowired / @Inject를 어디에 붙일까?

- 생성자
- 필드
- Setter

# AOP 소개

Aspected-Oriented Programming

흩어진 코드를 한 곳으로 모아

### 흩어진 AAAA 와 BBBB

```java
class A {
	method a () {
		AAAA -> AAA
		오늘은 7월 4일 미국 독립 기념일이래요.
		BBBB -> BB
	}

	method b () {
		AAAA -> AAA
		저는 아침에 운동을 다녀와서 밥먹고 빨래를 했습니다.
		BBBB -> BB
	}
}

class B {
	method c() {
		AAAA -> AAA
		점심은 이거 찍느라 못먹었는데 저녁엔 제육볶음을 먹고 싶네요.
		BBBB -> BB
	}
}
```

### 모아 놓은 AAAA 와 BBBB

```java
class A {

	method a () {
		오늘은 7월 4일 미국 독립 기념일이래요.
	}

	method b () {
		저는 아침에 운동을 다녀와서 밥먹고 빨래를 했습니다.
	}
}

class B {
	method c() {
		점심은 이거 찍느라 못먹었는데 저녁엔 제육볶음을 먹고 싶네요.
	}
}

class AAAABBBB {
	method aaaabbb(JoinPoint point) {
		AAAA
		point.execute()
		BBBB
	}
}
```

### 다양한 AOP 구현 방법

- 컴파일 A.java ----(AOP)---> A.class ([AspectJ](https://www.eclipse.org/aspectj/)) (컴파일 할때)
- 바이트코드 조작 A.java -> A.class ---(AOP)---> 메모리 (AspectJ) (읽어올때)
- 프록시 패턴 (스프링 AOP)

### 프록시 패턴

- [https://refactoring.guru/design-patterns/proxy](https://refactoring.guru/design-patterns/proxy)

# AOP 적용 예제

@LogExecutionTime 으로 메소드 처리 시간 로깅하기

```java
package org.springframework.samples.petclinic.owner;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

// @LogExecutionTime 애노테이션 (어디에 적용할지 표시 해두는 용도)
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface LogExecutionTime {
}
```

### 실제 Aspect (@LogExecutionTime 애노테이션 달린곳에 적용)

```java
@Component
@Aspect
public class LogAspect {
	Logger logger = LoggerFactory.*getLogger*(LogAspect.class);

	@Around("@annotation(LogExecutionTime)")
	public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
		StopWatch stopWatch = new StopWatch();
		stopWatch.start();

		Object proceed = joinPoint.proceed();

		stopWatch.stop();
		logger.info(stopWatch.prettyPrint());

		return proceed;
	}
}
```

# PSA

Portable Service Abstraction

잘 만든 인터페이스

### Service Abstraction

[https://en.wikipedia.org/wiki/Service_abstraction](https://en.wikipedia.org/wiki/Service_abstraction)

### 스프링 웹 MVC

@Controller 와 @RequestMapping

## 스프링 트랜잭션

PlatformTransactionManager

### @Transactional

all or nothing

## 스프링 캐시

CacheManager
