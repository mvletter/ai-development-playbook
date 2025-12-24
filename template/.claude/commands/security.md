# /security

Security audit and generate a feature spec for fixes.

## Instructions

Read these docs:
- docs/architecture.md (system boundaries)
  - If split: also read docs/architecture/integrations.md for external APIs
- docs/database.md (data model)
- docs/pitfalls.md (known security pitfalls)

## Analysis protocol (30 minutes)

### Phase 1: Attack surface

**Entry points:**
- HTTP endpoints - auth requirements?
- File upload/download - validation?
- Database queries - parameterized?
- External API calls - credentials exposed?
- WebSocket - authenticated?

**Sensitive data:**
- Customer data (PII)
- Tokens and sessions
- API keys
- Internal configs

### Phase 2: Code review

**Input validation:**
- User input sanitized?
- File paths validated?
- SQL parameterized?

**Auth:**
- Token generation secure?
- Session management correct?
- Permission checks on all routes?

**Data protection:**
- Encryption at rest/transit?
- Sensitive data in logs?

### Phase 3: Configuration

- Secrets in env vars (not code)?
- Security headers?
- CORS restricted?
- Rate limiting?
- Debug disabled in prod?

### Phase 4: Dependencies

- Known vulnerabilities?
- Outdated packages?

### Prioritize

**Critical**: Active vulnerabilities, exposed credentials
**High**: Missing validation, weak auth
**Medium**: Hardening, monitoring
**Low**: Best practices

## Output

Create `docs/features/security-[YYYY-MM-DD].md` using feature template.

Include:
- Critical and high findings with impact
- Tasks ordered by priority
- Security tests for each fix

Update project.md with link under TODO.
Update claude-progress.txt with audit status.

## Do NOT

- Ignore critical findings
- Fix without tests
- Store secrets in code
- Skip "internal only" findings
