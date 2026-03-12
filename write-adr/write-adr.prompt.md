---
mode: ask
description: Generate an Architecture Decision Record (ADR) for the current codebase
---

Write an Architecture Decision Record (ADR) for the following decision:

**Decision:** ${input:decision:Describe the architectural decision to document}

Use this structure:

```markdown
# ADR-NNN: <title>

**Date:** <today's date>
**Status:** Proposed | Accepted | Deprecated | Superseded

## Context
<What is the issue motivating this decision? What forces are at play?>

## Decision
<What is the change that we're proposing or have agreed to implement?>

## Consequences

### Positive
- <Good things that will result from this decision>

### Negative
- <Drawbacks or trade-offs introduced>

### Neutral
- <Things that change but are neither good nor bad>

## Alternatives Considered
<What other options were evaluated and why were they rejected?>
```

Base the ADR on the actual codebase context. Reference specific files, libraries, or patterns already in use where relevant. Keep the tone technical but concise — each section should be 2–5 bullet points or sentences.
