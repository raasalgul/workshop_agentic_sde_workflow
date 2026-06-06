# Project Instructions

This repository is used for a live AI agent workshop around a Java/Spring/Vaadin rent dashboard and template-engine workflow. Optimize for clear demonstrations, repeatable Gradle commands, and code that compiles on the first try.

## Default Behavior

- Read the existing code and lazy-load only the `.github/skills/**` guidance relevant to the task before implementing.
- Treat `.github/docs/scratch_book.txt` as the canonical workshop requirement scratch pad.
- Store local issue drafts in `.github/docs/issues/`.
- Prefer small vertical slices that can be explained and validated live.
- Keep examples realistic for an Ireland rent dashboard: counties, listings, rents, property summaries, dashboards, filters, and admin workflows.
- Use Java 21, Gradle, Spring Boot 3.x, Vaadin Flow, JPA/Hibernate, Flyway/Liquibase, JUnit 5, AssertJ, and Mockito.
- Use Gradle as the workshop build tool. Do not introduce Maven.
- Follow the repo's package layout over any generic template.

## Skill Routing

- Skills are lazy-loaded: start from the relevant skill's `SKILL.md`, then open only the referenced files needed for the current task.
- Use `java-architect` for Spring Boot, JPA, security, WebFlux, and general enterprise Java design.
- Use `java-vaadin-template-engine` for Vaadin Flow route/view/form/grid generation and template-engine scaffolding.
- Use `workshop-demo-guardian` before live runs, demos, dependency changes, or broad refactors.

## Product Management Flow

- Product requirements enter through `.github/docs/scratch_book.txt`.
- The Product Manager Agent must read that file before asking questions or creating issue drafts.
- Issue drafts must be Markdown files under `.github/docs/issues/`, named `NNN-short-kebab-title.md`.
- Remote GitHub issues are outside the default workflow. Create them only when explicitly requested.

## Coding Standards

- Keep Vaadin views thin; call application services instead of repositories directly.
- Put business rules in services/domain classes, not UI event handlers.
- Use DTOs/records for read models and commands where practical.
- Add Bean Validation annotations for user input.
- Add database migrations for schema changes.
- Externalize secrets and environment-specific values.
- Do not add dependencies during the live workshop. Add one only after an explicit user request.

## Verification Standard

For every code change, run validation in this order:

1. Run `./gradlew compileJava`.
2. If compile fails, fix code and rerun `./gradlew compileJava`.
3. After compile passes, run `./gradlew test`.
4. After tests pass, run `./gradlew check`.
5. For UI work, run `./gradlew bootRun` and smoke-test the target route.

If a command cannot run because the app has not been generated yet or a local dependency is unavailable, record the exact blocker and the next best validation completed.

## Live Workshop Bias

- Keep commands copy-pasteable.
- Prefer known-good local data over live services.
- Include clear fallbacks for ports and frontend dependency downloads.
- Do not leave the project in a half-generated state.
- Summaries should tell the presenter what changed, what passed, what did not run, and what to do live.
