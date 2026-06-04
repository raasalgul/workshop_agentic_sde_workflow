---
name: java-vaadin-template-engine
description: Use when creating, modifying, or debugging a Java Vaadin Flow template engine or generated Vaadin CRUD/dashboard features. Invoke for Spring Boot + Vaadin route generation, MainLayout integration, Binder forms, Grid views, DTO/service scaffolding, theme usage, security annotations, and workshop-safe generated code.
metadata:
  version: "1.0.0"
  domain: java-ui
  triggers: Vaadin, Flow, template engine, generator, scaffold, CRUD, dashboard, Spring Boot UI
  related-skills: java-architect, workshop-demo-guardian
---

# Java Vaadin Template Engine

Build deterministic, runnable Vaadin Flow features for Spring Boot projects. Favor predictable generated code over clever abstractions.

## Workflow

1. **Inspect before generating** - Read package structure, `build.gradle`, existing routes/layouts, security config, DTOs, services, and tests.
2. **Map the feature contract** - Identify entity/DTO names, route, fields, validation constraints, service methods, security role, and whether the output is CRUD, dashboard, search, or read-only.
3. **Generate a vertical slice** - View, form/component when the issue needs input UI, DTO/command record, service boundary, and tests.
4. **Wire into navigation** - Add a `SideNavItem`, `RouterLink`, menu entry, or route registration using the existing layout pattern.
5. **Validate** - Run `./gradlew compileJava`, `./gradlew test`, and `./gradlew check`. For UI changes, run `./gradlew bootRun` and smoke-test the route.

## Reference Files

- Load `../java-architect/references/vaadin.md` for Vaadin Flow patterns.
- Load `../java-architect/references/spring-security.md` for route security.
- Load `../java-architect/references/testing-patterns.md` for test strategy.

## Template Contract

Use this placeholder set for new templates:

```text
{{packageName}}         com.example
{{featureName}}         Property
{{featureNamePlural}}   Properties
{{route}}               properties
{{entityName}}          Property
{{summaryDto}}          PropertySummary
{{commandDto}}          PropertyCommand
{{serviceName}}         PropertyService
{{viewName}}            PropertiesView
{{formName}}            PropertyForm
{{layoutName}}          MainLayout
{{securityAnnotation}}  @PermitAll
{{fields}}              county:String,rent:BigDecimal
```

## Generation Rules

- Keep views thin; call services for business logic and persistence.
- Use `Grid<T>` for tabular data and `Binder<T>`/`BeanValidationBinder<T>` for forms.
- Prefer constructor injection for services.
- Use Java records for DTOs and command objects when compatible with Binder requirements. Use beans only when the existing Binder style needs mutable properties.
- Add explicit empty/error/user-feedback states with `Notification`, `Span`, or existing project components.
- Add route security with `@AnonymousAllowed`, `@PermitAll`, or `@RolesAllowed`.
- Do not overwrite existing files blindly. Patch existing layout/navigation and generate new files only for missing feature pieces.
- Avoid frontend build surprises: do not add npm packages during the workshop.

## Output Checklist

- Route compiles and has a stable `@Route`.
- Navigation points to the route.
- View uses existing theme/layout conventions.
- Form validates required fields and shows feedback.
- Service boundary exists and is testable.
- Security annotation is intentional.
- Tests or smoke checks cover route construction and service interaction.
- `./gradlew compileJava`, `./gradlew test`, and `./gradlew check` have been attempted with the exact result recorded.
