# Ireland Rent Dashboard Workshop

## Required Tools

Install these before the workshop:

1. **VS Code**
   - Recommended editor for using the custom agents in `.github/agents/`.
   - Install the GitHub Copilot / Copilot Chat extensions if your workshop flow uses VS Code chat agents.

2. **Java 21 JDK**
   - Required for Spring Boot 3.x and Vaadin Flow.
   - Verify:

   ```bash
   java -version
   ```

3. **Gradle**
   - Required to generate, build, and run the workshop app.
   - Use `./gradlew` after the project wrapper exists.
   - Verify:

   ```bash
   gradle --version
   ```

4. **Git**
   - Needed for version control and workshop checkpoints.
   - Verify:

   ```bash
   git --version
   ```

## Repository Purpose

This repository contains the workshop instructions, custom agents, and skills for building a Java/Spring/Vaadin Ireland rent dashboard and template-engine demo.

The important workshop files are:

- `.github/project-instructions.md` - Global project rules for agents.
- `.github/agents/software-engineer-agent.md` - Engineering agent behavior.
- `.github/agents/product-manager-agent.md` - Product/issue drafting agent behavior.
- `.github/skills/java-architect/` - Java, Spring Boot, JPA, security, testing, and Vaadin guidance.
- `.github/skills/java-vaadin-template-engine/` - Vaadin template-engine generation guidance.
- `.github/skills/workshop-demo-guardian/` - Live demo reliability guidance.
- `.github/docs/scratch_book.txt` - Requirement scratch pad.
- `.github/docs/issues/` - Local issue drafts created from the scratch book.

## How To Use The Custom Agents

### 1. Add Requirements

Write raw workshop requirements, feature ideas, constraints, or presenter notes in:

```text
.github/docs/scratch_book.txt
```

Use this file as the source of truth for product input.

### 2. Run The Product Manager Agent

Select **Product Manager Agent** in your VS Code/Copilot agent workflow and ask it to convert the scratch book into issue drafts.

Expected behavior:

- Reads `.github/docs/scratch_book.txt`.
- Splits requirements into small workshop-ready feature slices.
- Creates Markdown issue drafts in `.github/docs/issues/`.
- Uses file names like:

```text
.github/docs/issues/001-rent-summary-dashboard.md
```

Remote GitHub issues are not part of the default workshop flow. Create them only after an explicit request.

### 3. Run The Software Engineer Agent

Select **Software Engineer Agent** when implementing an issue draft.

Recommended prompt:

```text
Implement .github/docs/issues/001-rent-summary-dashboard.md using the project instructions and relevant skills.
```

Expected behavior:

- Reads `.github/project-instructions.md`.
- Loads relevant skills from `.github/skills/`.
- Uses `java-architect` for Java/Spring work.
- Uses `java-vaadin-template-engine` for Vaadin route, form, grid, and template generation.
- Uses `workshop-demo-guardian` for live-demo preflight and fallback planning.

## Build And Run Commands

Once the Java app exists, prefer the project wrapper.

For Gradle projects:

```bash
./gradlew compileJava
./gradlew test
./gradlew bootRun
```

Use installed Gradle only to generate or bootstrap the project before `./gradlew` exists.

## Live Workshop Flow

1. Put requirements in `.github/docs/scratch_book.txt`.
2. Ask **Product Manager Agent** to create issue drafts under `.github/docs/issues/`.
3. Pick one small issue for the live demo.
4. Ask **Software Engineer Agent** to implement that issue.
5. Run the focused compile/test command.
6. Start the app and smoke-test the Vaadin route.
7. Keep the full verification command for pre-demo and post-demo checks.

## Demo Safety Rules

- Keep features small and visible.
- Prefer local deterministic data over live services.
- Avoid adding new dependencies during the live session.
- Check ports before starting the app.
- If a command fails, record the exact blocker and continue with the safest local fallback.
