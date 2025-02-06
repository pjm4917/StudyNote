>[!SUMMARY]- 목차
>- [1. SLF4J (Simple Logging Facade for Java)](#1.%20SLF4J%20(Simple%20Logging%20Facade%20for%20Java))
>- [2. Logback](#2.%20Logback)
>- [3. SLF4J + Logback 사용법](#3.%20SLF4J%20+%20Logback%20%EC%82%AC%EC%9A%A9%EB%B2%95)
>- [4. SLF4J + Logback의 관계](#4.%20SLF4J%20+%20Logback%EC%9D%98%20%EA%B4%80%EA%B3%84)
>- [5. SLF4J + Logback의 장점](#5.%20SLF4J%20+%20Logback%EC%9D%98%20%EC%9E%A5%EC%A0%90)
>- [6. SLF4J vs Logback 비교](#6.%20SLF4J%20vs%20Logback%20%EB%B9%84%EA%B5%90)
>- [7. 결론](#7.%20%EA%B2%B0%EB%A1%A0)

## 1. SLF4J (Simple Logging Facade for Java)

SLF4J는 Java 애플리케이션에서 로그를 남길 때 사용할 수 있는 **로그 퍼사드(Facade, 인터페이스)** 입니다.

### 🔹 주요 특징

- SLF4J 자체는 **로깅 구현체가 아님** (API 역할만 수행)
- 실행 시점(runtime)에 적절한 **바인딩(binding) 라이브러리** 추가 필요
- `slf4j-api`를 사용하면 다양한 로깅 프레임워크(Logback, Log4j 등)로 쉽게 교체 가능
- **지연(Lazy) 평가 방식**으로 성능 최적화

### 🔹 SLF4J 코드 예제

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class Slf4jExample {
    private static final Logger logger = LoggerFactory.getLogger(Slf4jExample.class);

    public static void main(String[] args) {
        logger.info("Hello, SLF4J!");
        logger.debug("Debug log example");
        logger.error("An error occurred!");
    }
}
```

---

## 2. Logback

Logback은 **SLF4J의 기본 로깅 구현체**로, 성능과 유연성이 뛰어난 로깅 프레임워크입니다.

### 🔹 Logback의 특징

- **SLF4J와 완벽한 호환** (SLF4J의 기본 구현체)
- **Log4j보다 빠르고 가볍다**
- **다양한 설정 방식 지원** (XML, Groovy)
- **Rolling File Appender 지원** (자동 로그 파일 분할)

### 🔹 Logback 설정 파일 (logback.xml 예제)

```xml
<configuration>
    <!-- 콘솔에 로그 출력 -->
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <!-- 로그 레벨 설정 -->
    <root level="info">
        <appender-ref ref="STDOUT"/>
    </root>
</configuration>
```

---

## 3. SLF4J + Logback 사용법

### **로그 사용 코드**

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class LogbackExample {
    private static final Logger logger = LoggerFactory.getLogger(LogbackExample.class);

    public static void main(String[] args) {
        logger.info("Logback을 사용한 로그 메시지 출력");
        logger.debug("이 메시지는 debug 레벨입니다.");
        logger.error("에러 발생!", new RuntimeException("예외 예시"));
    }
}
```

---

## 4. SLF4J + Logback의 관계

- **SLF4J는 로깅 인터페이스, Logback은 로깅 구현체**
- SLF4J를 사용하면 **Logback, Log4j 등 다양한 로깅 프레임워크를 쉽게 변경 가능**
- Logback은 SLF4J의 **기본(default) 로깅 구현체**로 가장 많이 사용됨

---

## 5. SLF4J + Logback의 장점

✅ **유연한 로깅 구성** → 코드 수정 없이 로깅 프레임워크 변경 가능  
✅ **고성능 로깅** → Logback이 빠르고 효율적  
✅ **직관적인 설정** → XML 및 Groovy로 로그 설정 가능  
✅ **다양한 출력 지원** → 콘솔, 파일, 원격 서버 등 다양한 출력 지원

---

## 6. SLF4J vs Logback 비교

|항목|SLF4J|Logback|
|---|---|---|
|역할|로깅 인터페이스 (Facade)|로깅 구현체 (SLF4J의 기본 구현체)|
|특징|다양한 로깅 프레임워크를 추상화|성능이 뛰어나고 다양한 기능 제공|
|주요 라이브러리|`slf4j-api`|`logback-classic`, `logback-core`|
|설정|바인딩 라이브러리 필요|`logback.xml`로 설정|

---

## 7. 결론

- **SLF4J는 인터페이스**, Logback은 **구현체**
- SLF4J를 사용하면 **다양한 로깅 프레임워크(Logback, Log4j 등)로 쉽게 전환 가능**
- **Logback은 SLF4J의 기본 구현체로 가장 많이 사용됨**

➡ **SLF4J + Logback 조합이 Java 로깅의 사실상 표준** 🚀