>[!SUMMARY]- 목차
>- [[#🚀 1. Spring MVC (Model-View-Controller)|🚀 1. Spring MVC (Model-View-Controller)]]
>- [[#⚡ 2. Spring WebFlux|⚡ 2. Spring WebFlux]]
>- [[#🔍 MVC vs WebFlux 비교|🔍 MVC vs WebFlux 비교]]
>- [[#🔥 언제 MVC, 언제 WebFlux를 사용할까?|🔥 언제 MVC, 언제 WebFlux를 사용할까?]]
>- [[#🎯 결론|🎯 결론]]


## 🚀 1. Spring MVC (Model-View-Controller)
Spring MVC는 전통적인 **동기(Blocking) 방식**의 웹 프레임워크입니다.

### ✅ 특징
1. **Servlet 기반**
   - 내부적으로 **Servlet API**(DispatcherServlet)를 사용하여 동작합니다.
   - 요청이 들어오면 서블릿 스레드가 생성되고, 해당 요청을 처리합니다.

2. **Blocking I/O 방식**
   - 하나의 요청이 들어오면 **서버의 쓰레드가 할당**되고, 요청 처리가 끝날 때까지 해당 쓰레드는 블록됩니다.
   - 스레드 개수가 제한되어 있어 **동시 요청이 많아지면 성능이 저하**될 수 있습니다.

3. **Spring Boot에서 기본적으로 제공**
   - `spring-boot-starter-web`을 사용하면 기본적으로 Spring MVC가 활성화됩니다.

4. **JSP, Thymeleaf, Mustache 등 다양한 뷰 템플릿 엔진 지원**
   - 주로 **서버 사이드 렌더링(SSR, Server-Side Rendering)** 방식으로 웹 페이지를 생성하는데 사용됩니다.

### ✅ 코드 예제 (Spring MVC)
```java
@RestController
@RequestMapping("/users")
public class UserController {
    
    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        User user = userService.getUserById(id);
        return ResponseEntity.ok(user);
    }
}
```

---

## ⚡ 2. Spring WebFlux
Spring WebFlux는 **비동기(Non-Blocking) 방식**의 웹 프레임워크입니다.

### ✅ 특징
1. **Reactive Streams 기반**
   - **Reactor 라이브러리**(`Mono` 및 `Flux`)를 사용하여 데이터 스트림을 처리합니다.
   - **서블릿 대신 Netty**를 사용할 수 있어 가볍고 확장성이 뛰어납니다.

2. **Non-Blocking I/O 방식**
   - 하나의 쓰레드가 여러 요청을 **비동기적으로 처리**할 수 있어 고성능 애플리케이션에 적합합니다.
   - 요청이 들어오면 즉시 반환하고, 결과가 준비되면 **콜백 방식**으로 응답을 처리합니다.

3. **Spring Boot에서 `spring-boot-starter-webflux`를 사용**
   - `spring-boot-starter-webflux`를 추가하면 Spring MVC가 아닌 WebFlux가 기본적으로 활성화됩니다.

4. **Functional Endpoints 지원**
   - 기존 `@Controller` 방식뿐만 아니라 **함수형 라우팅**을 사용할 수 있습니다.

### ✅ 코드 예제 (Spring WebFlux)
```java
@RestController
@RequestMapping("/users")
public class UserController {
    
    @GetMapping("/{id}")
    public Mono<ResponseEntity<User>> getUser(@PathVariable Long id) {
        return userService.getUserById(id)
                .map(ResponseEntity::ok);
    }
}
```

---

## 🔍 MVC vs WebFlux 비교

| 항목             | Spring MVC | Spring WebFlux |
|-----------------|-----------|--------------|
| **기반**       | 서블릿 (Servlet) | 리액티브 스트림 (Reactive Streams) |
| **I/O 방식**   | 동기 (Blocking) | 비동기 (Non-Blocking) |
| **멀티스레드 방식** | 요청마다 새로운 스레드 할당 | 이벤트 루프 기반 처리 |
| **성능**       | 일반적인 트래픽에서는 좋음 | 고성능(동시 요청 많을 때 유리) |
| **적합한 환경** | REST API, 일반적인 웹 애플리케이션 | 대량의 트래픽 처리, 실시간 데이터 처리 |
| **사용하는 라이브러리** | `spring-boot-starter-web` | `spring-boot-starter-webflux` |
| **서버** | Tomcat (기본) | Netty (기본) |
| **데이터 처리 방식** | `List<T>` | `Flux<T>` (0~N개), `Mono<T>` (0~1개) |

---

## 🔥 언제 MVC, 언제 WebFlux를 사용할까?
✅ **Spring MVC를 선택해야 하는 경우**
- 기존의 **전통적인 웹 애플리케이션**을 개발할 때
- 블로킹 I/O 기반이지만, **대부분의 요청이 빠르게 처리되는 서비스** (예: 일반적인 CRUD API)
- 개발자가 WebFlux의 리액티브 패러다임에 익숙하지 않을 때

✅ **Spring WebFlux를 선택해야 하는 경우**
- **대량의 동시 요청을 처리**해야 하는 경우 (예: 실시간 채팅, 스트리밍 서비스)
- **마이크로서비스 및 비동기 API 연동**이 필요한 경우
- **WebSocket, SSE (Server-Sent Events)** 같은 실시간 데이터를 처리해야 할 때
- **MongoDB, Redis 등 리액티브 DB**와 함께 사용할 때

---

## 🎯 결론
- **일반적인 애플리케이션** → `Spring MVC` 사용
- **비동기 데이터 처리, 높은 트래픽** → `Spring WebFlux` 사용  
- **서로 다른 서비스 조합 가능** (e.g., WebFlux API를 호출하는 MVC 애플리케이션)  

Spring WebFlux는 강력하지만 기존 MVC보다 **학습 곡선이 가파를 수 있음**.  
따라서 **비동기 방식이 꼭 필요한 경우에만 WebFlux를 고려**하는 것이 좋습니다! 🚀
