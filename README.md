# Ireland Rent Dashboard Workshop

Welcome! This repository is the starting point for the workshop so attendees can follow the live coding flow, use VS Code agents, and work on the Ireland Rent Dashboard demo.

## What You Need Before the Workshop

1. **VS Code**
   - Use VS Code to access the custom agents and workshop files.
   - Install GitHub Copilot / Copilot Chat if your session uses the chat agent flow.

2. **Java 21 JDK**
   - Required for Spring Boot 3.x and Vaadin Flow.
   - Confirm with:

   ```bash
   java -version
   ```

3. **Gradle**
   - The project uses Gradle to build and run the app.
   - Use the wrapper once it exists: `./gradlew`
   - Confirm with:

   ```bash
   gradle --version
   ```

4. **Git**
   - Needed for version control, checkpoints, and workshop collaboration.
   - Confirm with:

   ```bash
   git --version
   ```

## How This Repo Works for the Workshop

This repository is designed around a simple feedback loop:

- capture requirements in a scratch pad
- convert them into issue drafts with a Product Manager Agent
- implement small issues with a Software Engineer Agent
- build, run, and verify the app locally

### Important Workshop Files

- `.github/docs/scratch_book.txt` — where attendees add raw feature ideas and requirements.
- `.github/docs/issues/` — where issue drafts are generated.
- `.github/project-instructions.md` — project rules used by the agents.
- `.github/agents/software-engineer-agent.md` — behavior guide for the engineering agent.
- `.github/agents/product-manager-agent.md` — behavior guide for the product agent.
- `.github/skills/java-architect/` — Java/Spring/Vaadin guidance.
- `.github/skills/java-vaadin-template-engine/` — Vaadin template generation guidance.
- `.github/skills/workshop-demo-guardian/` — live-demo reliability guidance.

## Attendee Workflow

1. Add workshop requirements to `.github/docs/scratch_book.txt`.
2. Use the **Product Manager Agent** to turn the scratch book into issue drafts.
3. Pick one issue from `.github/docs/issues/` to implement.
4. Use the **Software Engineer Agent** to implement that issue.
5. Run the app locally and verify the feature.

## Using the Product Manager Agent

The Product Manager Agent helps break large workshop goals into small, actionable issues.

- Open the agent in VS Code/Copilot.
- Ask it to convert `.github/docs/scratch_book.txt` into issue drafts.
- It should create files under `.github/docs/issues/` such as:

```text
.github/docs/issues/001-rent-summary-dashboard.md
```

> Note: This repo does not automatically create remote GitHub issues unless explicitly requested.

## Using the Software Engineer Agent

The Software Engineer Agent should implement issue drafts using the project rules and skill guidance.

Recommended prompt:

```text
Implement .github/docs/issues/001-rent-summary-dashboard.md using the project instructions and relevant skills.
```

Expected behavior:

- reads `.github/project-instructions.md`
- loads relevant skills from `.github/skills/`
- uses Java/Spring guidance from `java-architect`
- uses Vaadin guidance from `java-vaadin-template-engine`
- uses `workshop-demo-guardian` for safe live-demo choices

## Build and Run

Use the Gradle wrapper when available:

```bash
./gradlew compileJava
./gradlew test
./gradlew bootRun
```

If the wrapper is not present yet, use your installed Gradle only for project generation and setup.

## Live Demo Checklist

- Keep each feature small and easy to verify.
- Prefer local data and avoid external services.
- Do not add new dependencies during live coding unless necessary.
- Check that the app port is free before starting the server.
- If a step fails, capture the exact error and switch to the safest fallback.

## Quick Start for Workshop Attendees

1. Open the workspace in VS Code.
2. Confirm Java, Gradle, and Git are installed.
3. Add your first idea to `.github/docs/scratch_book.txt`.
4. Run the Product Manager Agent to create an issue draft.
5. Ask the Software Engineer Agent to implement that issue.
6. Build and run the app to verify the result.

Enjoy the workshop and use this repo as your shared starting point for the live session.