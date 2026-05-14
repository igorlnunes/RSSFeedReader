# RSS Feed Reader Constitution

## Sync Impact Report

<!-- HTML comment block for impact tracking -->

**Version Change**: 0.0.0 → 1.0.0 (MINOR: Initial constitution with 5 core principles)

**Modified Principles**: N/A (initial version)

**Added Sections**:

- Security Requirements (input validation, XSS prevention)
- Development Workflow (testing, code review, documentation)

**Removed Sections**: None

**Templates Updated**:

- ⚠ Pending: `.specify/templates/plan-template.md`
- ⚠ Pending: `.specify/templates/spec-template.md`
- ⚠ Pending: `.specify/templates/tasks-template.md`

## Core Principles

### I. Security First

Input validation is mandatory for all API endpoints and user inputs. Feed URLs MUST be validated before processing. XSS prevention is non-negotiable: all feed content (Extended-MVP) must be sanitized before rendering. API endpoints MUST enforce CORS correctly, rejecting unauthenticated cross-origin requests. No sensitive data (URLs, user actions) should be logged in plaintext.

### II. Separation of Concerns

Backend (ASP.NET Core API) is responsible for feed operations and data management; Frontend (Blazor WebAssembly) is responsible for user interaction and display logic. Each layer must be independently testable. Shared models are permitted only when necessary for API contracts; UI-specific models belong in the frontend. Database operations (Extended-MVP+) remain isolated in a persistence layer.

### III. Test-Driven Development (NON-NEGOTIABLE)

All new features and bug fixes MUST follow TDD: write tests first, user approval on expected behavior, confirm tests fail, then implement. Unit tests cover individual components (API endpoints, UI components, feed parsers). Integration tests verify backend-to-frontend communication, especially subscription management and (Extended-MVP) feed fetching. Minimum coverage target: 70% for non-UI code, 50% for UI.

### IV. Incremental Delivery & Clear Scope

MVP phase focuses solely on subscription management (add feed, display list); no feed fetching, parsing, or validation. Extended-MVP adds feed fetching and display; no persistence or removal yet. Post-MVP phases follow. Each phase is complete and working before the next begins. Feature scope must be explicitly documented in tickets; creeping scope violates this principle.

### V. Maintainability Through Documentation

Every public API endpoint MUST have XML documentation comments. Complex business logic (especially feed parsing in Extended-MVP) requires inline code comments explaining the "why". Configuration (backend URLs, API ports, CORS origins) is externalized in `appsettings.json` and documented in `README.md` or `docs/` folder. All significant design decisions are captured in ADRs (Architecture Decision Records) or commit messages.

## Security Requirements

**Input Validation**:

- Feed URLs MUST be validated: test that they conform to URI standards before storage or processing.
- Subscription input fields MUST reject obviously invalid data (empty strings, malicious payloads).
- No direct SQL queries; all database access (Extended-MVP+) uses Entity Framework Core.

**Cross-Site Scripting (XSS) Prevention**:

- All feed content displayed in the UI (titles, descriptions, links in Extended-MVP) must be escaped or rendered as plain text.
- Blazor components automatically escape content by default; raw HTML rendering is forbidden without explicit security review.

**API Security**:

- CORS MUST be configured to allow only the frontend origin (localhost during dev, production domain in production).
- No authentication required for MVP; future features may require token-based auth.
- Error responses MUST NOT expose internal system details (file paths, stack traces, database structure).

## Development Workflow

**Code Review & Quality Gate**:

- All changes (except documentation-only commits) require peer review before merge.
- Reviews MUST verify: test coverage, no security violations, adherence to documentation standards, no console errors in deployment.

**Testing Requirements**:

- Unit tests run on every commit; build fails if tests fail.
- Integration tests for new API endpoints and UI interactions.
- Manual testing against acceptance criteria before marking as "done".

**Branching & Commits**:

- Feature branches follow naming convention: `feature/short-description` or `fix/bug-description`.
- Commit messages follow convention: `type(scope): brief description` (e.g., `feat(api): add subscription endpoint`).
- Commits are atomic and compilable; avoid "WIP" commits in final PR.

**Documentation Commits**:

- Documentation updates (README, ADRs) can be committed separately but should accompany feature commits when they explain the feature.

## Governance

This Constitution supersedes all other informal practices. All decisions about code structure, testing, and security must align with these principles.

**Amendment Process**: Changes to this Constitution require ratification (decision by lead developer or team consensus). Each amendment increments the version number and documents the change date.

**Compliance Verification**: Pull request reviews MUST explicitly verify compliance with Principles I, III, and the Security/Testing sections. Violations block merge until resolved.

**Runtime Guidance**: For day-to-day development questions not covered here, refer to the project README, TechStack.md, and feature specifications in `.specify/memory/spec.md`.

**Version**: 1.0.0 | **Ratified**: 2026-05-14 | **Last Amended**: 2026-05-14
