# Research: Technical Decisions for MVP RSS Reader

## Decision: Language / Version

- Decision: Use .NET 8 (C# 12) for backend and Blazor WebAssembly for frontend.
- Rationale: Aligns with Stakeholder TechStack (ASP.NET Core + Blazor). .NET 8 is current LTS-capable runtime with long-term support and good Blazor tooling as of 2026.
- Alternatives considered: .NET 7 (shorter support horizon), Node/React (more ecosystem but diverges from stakeholder choice).

## Decision: Primary Dependencies

- Decision: Backend: ASP.NET Core Web API; Frontend: Blazor WebAssembly; Testing: xUnit; (Extended-MVP) feed parsing via System.ServiceModel.Syndication.
- Rationale: Matches TechStack.md and enables code sharing in C#. xUnit integrates with .NET test workflows.
- Alternatives: Use third-party feed parsers (e.g., CodeHollow) for advanced parsing; reserved for Extended-MVP.

## Decision: Storage

- Decision: In-memory storage for MVP (List-backed repository). Extended-MVP will add EF Core + SQLite if persistence required.
- Rationale: Meets the rapid-delivery objective, minimizes setup and cross-platform friction during demos.
- Alternatives: SQLite from the start — rejected to keep MVP minimal.

## Decision: Testing

- Decision: Use xUnit for unit tests and minimal integration tests for the API endpoints. Follow TDD practice at minimal level: write tests for `AddSubscription` and `GetSubscriptions` endpoints before implementation.
- Rationale: Constitution requires TDD. Minimal tests satisfy gating while keeping effort small.
- Alternatives: NUnit/MSTest — xUnit is standard in many .NET projects.

## Decision: Target Platform

- Decision: Cross-platform (Linux, Windows, macOS). Development on Linux is supported; CI should run Linux runners.
- Rationale: Stakeholder goals and project's cross-platform requirement.

## Decision: Project Type & Structure

- Decision: Web application with separate `backend/` (ASP.NET Core API) and `frontend/` (Blazor WASM) projects.
- Rationale: Matches TechStack guidance and separation-of-concerns constitution principle.
- Alternatives: Single-project minimal server-rendered app — rejected because Blazor WASM is requested.

## Decision: Performance Goals / Constraints

- Decision: No specific performance targets for MVP beyond responsive UI for small local datasets. Keep memory and latency expectations minimal (in-memory lists, sub-second response for local calls).
- Rationale: MVP scope is tiny; premature optimization is unnecessary.

## Decision: Scale / Scope

- Decision: Designed for single-user local demos. Scale to multi-user or large-scale not in scope for MVP.

## Constitution Implications

- Security: Even though MVP treats subscriptions in-memory, validate user input (non-empty, well-formed URL) to satisfy the Constitution's Input Validation principle.
- XSS: Render any feed content as plain text (MVP avoids fetching/displaying HTML). Sanitize or escape in Extended-MVP if raw HTML is shown.
- TDD: Implement minimal unit tests before writing production code for the core API endpoints.

## Conclusion

These decisions resolve the Technical Context placeholders in the plan and allow Phase 1 design work to proceed (data-model.md, quickstart.md, contracts). If you prefer a different .NET version (e.g., .NET 9), confirm and I will adjust the plan artifacts accordingly.
