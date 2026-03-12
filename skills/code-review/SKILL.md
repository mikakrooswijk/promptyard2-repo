# Code Review Skill

## Purpose
Perform thorough, constructive code reviews focused on correctness, maintainability, and security.

## Review Checklist

### Correctness
- Does the logic match the stated intent?
- Are edge cases handled (null, empty, overflow)?
- Are error paths covered and tested?

### Security
- No hardcoded secrets or credentials
- Input validation at system boundaries
- No SQL/command injection surface
- Dependencies are up-to-date and trusted

### Maintainability
- Functions have a single responsibility
- Names clearly describe what they do
- No dead code or commented-out blocks
- Complex logic has a brief comment explaining *why*

### Tests
- New behaviour is covered by tests
- Tests are deterministic and isolated
- Coverage for happy path and error cases

## Output Format
For each finding use:
```
[severity] file.ts#L42 — short description
```
Severities: **blocking**, **warning**, **nit**

Conclude with a summary sentence and an overall recommendation: `approve`, `request-changes`, or `needs-discussion`.
