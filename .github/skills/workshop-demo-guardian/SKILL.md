---
name: workshop-demo-guardian
description: Use before or during a live coding workshop, demo, or training session to make Java/Spring/Vaadin work reliable. Invoke for preflight checks, fallback plans, fast validation commands, dependency risk reduction, demo scripts, recovery steps, and "do not fail live" hardening.
metadata:
  version: "1.0.0"
  domain: reliability
  triggers: workshop, demo, live coding, preflight, backup plan, dry run, fail-safe
  related-skills: java-architect, java-vaadin-template-engine
---

# Workshop Demo Guardian

Make the live path boring in the best possible way: known commands, known outputs, graceful fallbacks.

## Preflight Workflow

1. **Inventory** - Confirm Java 21, Gradle, app entry point, port, local data source, env vars, and internet-dependent steps.
2. **Run the happy path** - Compile, run narrow tests, start the app, open the target page/API, and capture the command sequence.
3. **Remove live risks** - Pre-download dependencies, seed local data, avoid first-time npm/frontend downloads, and document required ports.
4. **Prepare fallbacks** - Keep a small known-good feature branch/patch, a seed dataset, and a short explanation path if a dependency fails.
5. **Write the operator script** - Provide copy-paste commands in the order the presenter should run them.

## Reliability Rules

- Prefer local deterministic data over live network calls.
- Prefer narrow tests during the workshop; reserve full verification for before and after the session.
- Do not introduce new dependencies on the day of the workshop.
- Keep generated examples small enough to explain in under five minutes.
- Include a rollback plan for any file edits made during the session.
- Check ports before starting servers. If the default port is busy, use an explicit alternate port and state it.

## Java/Vaadin Checks

```bash
java -version
./gradlew --version
./gradlew compileJava
./gradlew test
./gradlew check
./gradlew bootRun
```

## Handoff Format

When preparing workshop material, end with:

```markdown
## Live Commands
[ordered commands]

## Expected Checks
[what should appear in logs/browser/tests]

## Fallbacks
[known issue] -> [recovery action]

## Do Not Do Live
[network-heavy or risky operations]
```
