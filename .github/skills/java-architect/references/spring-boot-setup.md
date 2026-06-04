# Spring Boot Setup

## Project Structure (Clean Architecture)

```
src/main/java/com/example/
├── domain/              # Core business logic
│   ├── model/          # Entities, value objects
│   ├── repository/     # Repository interfaces
│   └── service/        # Domain services
├── application/         # Use cases
│   ├── dto/            # Request/Response DTOs
│   ├── mapper/         # Entity <-> DTO mappers
│   └── service/        # Application services
├── infrastructure/      # External concerns
│   ├── persistence/    # JPA implementations
│   ├── config/         # Spring configuration
│   └── security/       # Security setup
└── presentation/        # API layer
    └── rest/           # REST controllers
```

## Modern build.gradle (Spring Boot 3.3+)

```groovy
plugins {
    id 'java'
    id 'org.springframework.boot' version '3.3.5'
    id 'io.spring.dependency-management' version '1.1.6'
}

group = 'com.example'
version = '1.0.0'

java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(21)
    }
}

repositories {
    mavenCentral()
}

ext {
    mapstructVersion = '1.5.5.Final'
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-validation'
    implementation 'org.flywaydb:flyway-core'
    implementation "org.mapstruct:mapstruct:${mapstructVersion}"

    runtimeOnly 'com.h2database:h2'

    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'
    annotationProcessor "org.mapstruct:mapstruct-processor:${mapstructVersion}"

    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

tasks.named('test') {
    useJUnitPlatform()
}
```

## Application Configuration

```yaml
# application.yml
spring:
  application:
    name: demo-service

  datasource:
    url: ${DATABASE_URL:jdbc:postgresql://localhost:5432/demo}
    username: ${DATABASE_USER:demo}
    password: ${DATABASE_PASSWORD:demo}
    hikari:
      maximum-pool-size: 10
      minimum-idle: 5
      connection-timeout: 20000

  jpa:
    hibernate:
      ddl-auto: validate
    open-in-view: false
    properties:
      hibernate:
        jdbc:
          batch_size: 20
        order_inserts: true
        order_updates: true

  flyway:
    enabled: true
    baseline-on-migrate: true
    locations: classpath:db/migration

server:
  port: 8080
  shutdown: graceful
  error:
    include-message: always
    include-binding-errors: always

management:
  endpoints:
    web:
      exposure:
        include: health,info,metrics,prometheus
  endpoint:
    health:
      show-details: when-authorized
  metrics:
    export:
      prometheus:
        enabled: true
```

## Main Application Class

```java
package com.example;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.data.jpa.repository.config.EnableJpaAuditing;

@SpringBootApplication
@EnableJpaAuditing
public class DemoServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoServiceApplication.class, args);
    }
}
```

## Configuration Classes

```java
package com.example.infrastructure.config;

import io.swagger.v3.oas.models.OpenAPI;
import io.swagger.v3.oas.models.info.Info;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class OpenApiConfig {

    @Bean
    public OpenAPI customOpenAPI() {
        return new OpenAPI()
            .info(new Info()
                .title("Demo Service API")
                .version("1.0.0")
                .description("Enterprise microservice API"));
    }
}
```

## Exception Handling

```java
package com.example.infrastructure.config;

import org.springframework.http.HttpStatus;
import org.springframework.http.ProblemDetail;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

import java.time.Instant;

@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(EntityNotFoundException.class)
    public ProblemDetail handleNotFound(EntityNotFoundException ex) {
        ProblemDetail problem = ProblemDetail.forStatusAndDetail(
            HttpStatus.NOT_FOUND,
            ex.getMessage()
        );
        problem.setProperty("timestamp", Instant.now());
        return problem;
    }

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ProblemDetail handleValidation(MethodArgumentNotValidException ex) {
        ProblemDetail problem = ProblemDetail.forStatusAndDetail(
            HttpStatus.BAD_REQUEST,
            "Validation failed"
        );
        problem.setProperty("errors", ex.getBindingResult()
            .getFieldErrors()
            .stream()
            .map(e -> e.getField() + ": " + e.getDefaultMessage())
            .toList());
        return problem;
    }
}
```

## Quick Reference

| Component | Purpose |
|-----------|---------|
| `@SpringBootApplication` | Main application entry point |
| `@Configuration` | Configuration classes |
| `@Bean` | Bean factory method |
| `@Value` | Inject properties |
| `@ConfigurationProperties` | Type-safe config |
| `@Profile` | Environment-specific beans |
| `@EnableJpaAuditing` | Automatic audit fields |
| `ProblemDetail` | RFC 7807 error responses |
