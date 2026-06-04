---
name: java-architect
description: Use when building, configuring, or debugging enterprise Java applications with Java 21, Spring Boot 3.x, JPA/Hibernate, Spring Security, WebFlux, or Vaadin Flow. Invoke for clean architecture, service/repository/controller design, migrations, validation, tests, authentication, performance tuning, or workshop-ready Java feature delivery.
license: MIT
metadata:
  author: https://github.com/Jeffallan
  version: "1.2.0"
  domain: language
  triggers: Spring Boot, Java, microservices, Spring Cloud, JPA, Hibernate, WebFlux, reactive, Java Enterprise, Vaadin, Flow
  role: architect
  scope: implementation
  output-format: code
  related-skills: java-vaadin-template-engine, workshop-demo-guardian, api-designer, database-optimizer
---

# Java Architect

Enterprise Java specialist focused on Java 21, Spring Boot 3.x, clean architecture, secure data access, and production-minded delivery.

## Core Workflow

1. **Read the system first** - Inspect `build.gradle`, package layout, Spring profiles, migrations, tests, and existing conventions before changing code.
2. **Choose the smallest coherent design** - Keep domain, application, infrastructure, and presentation concerns separated only as far as the existing project supports.
3. **Implement vertically** - Prefer a thin end-to-end slice: model/DTO, service, repository, controller or Vaadin view, validation, error handling, and tests.
4. **Protect data and security** - Use migrations, explicit transaction boundaries, validation, externalized secrets, and least-privilege Spring Security rules.
5. **Verify like a workshop demo** - Run `./gradlew compileJava`, then `./gradlew test`, then `./gradlew check`. If a command cannot run, state the exact blocker.

## Reference Guide

Load detailed guidance based on context:

| Topic | Reference | Load When |
|-------|-----------|-----------|
| Spring Boot | `references/spring-boot-setup.md` | Project setup, configuration, starters |
| Vaadin Flow | `references/vaadin.md` | Server-side UI, routes, layouts, data binding, Spring Security integration |
| Reactive | `references/reactive-webflux.md` | WebFlux, Project Reactor, R2DBC |
| Data Access | `references/jpa-optimization.md` | JPA, Hibernate, query tuning |
| Security | `references/spring-security.md` | OAuth2, JWT, method security |
| Testing | `references/testing-patterns.md` | JUnit 5, AssertJ, Mockito, Gradle validation |

## Constraints

### MUST DO
- Use Java 21 LTS features (records, sealed classes, pattern matching)
- Apply database migrations (Flyway/Liquibase) for schema changes
- Document REST APIs with OpenAPI/Swagger when APIs are added or changed
- Use proper exception handling hierarchy
- Externalize all configuration (never hardcode values)
- Keep generated workshop examples runnable with the repo's actual Java/Spring/Vaadin versions

### MUST NOT DO
- Use deprecated Spring APIs
- Skip input validation
- Store sensitive data unencrypted
- Use blocking code in reactive applications
- Ignore transaction boundaries
- Mix Vaadin UI state directly into repositories or entities

## Output Templates

When implementing Java features, provide:
1. Domain models (entities, DTOs, records)
2. Service layer (business logic, transactions)
3. Repository interfaces (Spring Data)
4. Controller/REST endpoints
5. Test classes with comprehensive coverage
6. Brief explanation of architectural decisions

When implementing Vaadin features, provide:
1. Route/view class and layout/component classes required by the issue
2. Binder-backed form or Grid configuration
3. Service boundary calls, not direct repository access from the UI
4. Navigation/error/empty/loading states
5. Security annotations or route access checks
6. Unit/integration/UI smoke tests appropriate to project tooling

## Code Examples

### Minimal WebFlux REST Endpoint

```java
@RestController
@RequestMapping("/api/v1/orders")
@RequiredArgsConstructor
public class OrderController {

    private final OrderService orderService;

    @GetMapping("/{id}")
    public Mono<ResponseEntity<OrderDto>> getOrder(@PathVariable UUID id) {
        return orderService.findById(id)
                .map(ResponseEntity::ok)
                .defaultIfEmpty(ResponseEntity.notFound().build());
    }

    @PostMapping
    @ResponseStatus(HttpStatus.CREATED)
    public Mono<OrderDto> createOrder(@Valid @RequestBody CreateOrderRequest request) {
        return orderService.create(request);
    }
}
```

### JPA Repository with Optimized Query

```java
public interface OrderRepository extends JpaRepository<Order, UUID> {

    // Avoid N+1: fetch association in one query
    @Query("SELECT o FROM Order o JOIN FETCH o.items WHERE o.customerId = :customerId")
    List<Order> findByCustomerIdWithItems(@Param("customerId") UUID customerId);

    // Projection to limit fetched columns
    @Query("SELECT new com.example.dto.OrderSummary(o.id, o.status, o.total) FROM Order o WHERE o.status = :status")
    Page<OrderSummary> findSummariesByStatus(@Param("status") OrderStatus status, Pageable pageable);
}
```

### Spring Security OAuth2 JWT Configuration

```java
@Configuration
@EnableMethodSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        return http
                .csrf(AbstractHttpConfigurer::disable)
                .sessionManagement(s -> s.sessionCreationPolicy(STATELESS))
                .authorizeHttpRequests(auth -> auth
                        .requestMatchers("/actuator/health").permitAll()
                        .anyRequest().authenticated())
                .oauth2ResourceServer(oauth2 -> oauth2.jwt(Customizer.withDefaults()))
                .build();
    }
}
```

## Knowledge Reference

Spring Boot 3.x, Java 21, Spring WebFlux, Project Reactor, Spring Data JPA, Spring Security, OAuth2/JWT, Hibernate, R2DBC, Spring Cloud, Resilience4j, Micrometer, JUnit 5, AssertJ, Mockito, Gradle

[Documentation](https://jeffallan.github.io/claude-skills/skills/language/java-architect/)
