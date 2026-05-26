# Tasks: MVP RSS reader

**Input**: Design documents from `/specs/001-mvp-rss-reader/`
**Prerequisites**: plan.md, spec.md, research.md, data-model.md, contracts/, quickstart.md

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Initialize the backend and frontend projects, and configure development settings.

- [ ] T001 Create backend project skeleton in `backend/RSSFeedReader.Api` using .NET 8 API conventions
- [ ] T002 Create frontend Blazor WebAssembly project skeleton in `frontend/RSSFeedReader.UI` using .NET 8
- [ ] T003 [P] Add or verify `specs/001-mvp-rss-reader/quickstart.md` contains MVP run instructions and backend/frontend startup commands
- [ ] T004 [P] Configure backend development CORS and default API base URL in `backend/RSSFeedReader.Api/appsettings.Development.json`

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Implement core data structures, repository, API contract, and frontend shell required by all user stories.

- [ ] T005 Create `Subscription` model and DTO in `backend/RSSFeedReader.Api/Models/Subscription.cs`
- [ ] T006 Create in-memory subscription repository in `backend/RSSFeedReader.Api/Repositories/InMemorySubscriptionRepository.cs`
- [ ] T007 Create subscription service in `backend/RSSFeedReader.Api/Services/SubscriptionService.cs`
- [ ] T008 Create backend API controller for `GET /api/subscriptions` and `POST /api/subscriptions` in `backend/RSSFeedReader.Api/Controllers/SubscriptionsController.cs`
- [ ] T009 Create frontend subscription list and add-subscription form shell in `frontend/RSSFeedReader.UI/Pages/Subscriptions.razor`
- [ ] T010 Create frontend subscription API client service in `frontend/RSSFeedReader.UI/Services/SubscriptionApiService.cs`

---

## Phase 3: User Story 1 - Add a feed subscription (Priority: P1) 🎯 MVP

**Goal**: Allow users to enter a valid RSS/Atom feed URL and save it as a subscription.

**Independent Test**: Add a valid feed URL and confirm the new subscription appears in the list with its title or URL.

### Implementation for User Story 1

- [ ] T011 [P] [US1] Implement backend subscription URL validation and duplicate detection in `backend/RSSFeedReader.Api/Services/SubscriptionService.cs`
- [ ] T012 [US1] Implement `POST /api/subscriptions` to add subscriptions and return `201 Created`, `400 Bad Request`, or `409 Conflict` in `backend/RSSFeedReader.Api/Controllers/SubscriptionsController.cs`
- [ ] T013 [US1] Implement `GET /api/subscriptions` in `backend/RSSFeedReader.Api/Controllers/SubscriptionsController.cs`
- [ ] T014 [P] [US1] Implement frontend add-subscription form submit flow in `frontend/RSSFeedReader.UI/Pages/Subscriptions.razor`
- [ ] T015 [P] [US1] Implement frontend subscription list rendering and refresh logic in `frontend/RSSFeedReader.UI/Pages/Subscriptions.razor`

**Checkpoint**: US1 should be fully functional and independently testable after these tasks.

---

## Phase 4: User Story 2 - View latest items from a subscribed feed (Priority: P2)

**Goal**: Show the latest published entries for a selected subscription.

**Independent Test**: Select a subscription and confirm at least one feed item is displayed with title and published date.

### Implementation for User Story 2

- [ ] T016 [P] [US2] Create `FeedItem` model in `backend/RSSFeedReader.Api/Models/FeedItem.cs`
- [ ] T017 [US2] Implement feed fetcher service in `backend/RSSFeedReader.Api/Services/FeedFetcherService.cs`
- [ ] T018 [US2] Add backend endpoint `GET /api/subscriptions/{id}/items` in `backend/RSSFeedReader.Api/Controllers/SubscriptionsController.cs`
- [ ] T019 [US2] Implement frontend selected-subscription detail page in `frontend/RSSFeedReader.UI/Pages/SubscriptionDetails.razor`
- [ ] T020 [P] [US2] Implement frontend feed item display and backend call in `frontend/RSSFeedReader.UI/Services/SubscriptionApiService.cs`

**Checkpoint**: US2 should be independently testable once the selected subscription details and item list are working.

---

## Phase 5: User Story 3 - Handle invalid feed URL input (Priority: P3)

**Goal**: Detect invalid or unsupported feed URLs and show a clear error without adding the subscription.

**Independent Test**: Enter an invalid feed URL and confirm the app displays a clear error and does not add the subscription.

### Implementation for User Story 3

- [ ] T021 [US3] Add backend validation for malformed URLs and invalid feed responses in `backend/RSSFeedReader.Api/Services/SubscriptionService.cs`
- [ ] T022 [US3] Add duplicate feed handling returning `409 Conflict` in `backend/RSSFeedReader.Api/Controllers/SubscriptionsController.cs`
- [ ] T023 [US3] Add frontend validation and user-visible error message handling in `frontend/RSSFeedReader.UI/Pages/Subscriptions.razor`
- [ ] T024 [P] [US3] Add frontend network and feed-fetch error handling in `frontend/RSSFeedReader.UI/Pages/Subscriptions.razor`

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: Finish documentation, cleanup, and final verification across the MVP.

- [ ] T025 [P] Update `specs/001-mvp-rss-reader/quickstart.md` with exact backend/frontend run instructions and API base URL guidance
- [ ] T026 [P] Add or refine manual acceptance notes in `specs/001-mvp-rss-reader/quickstart.md` for US1, US2, and US3

---

## Dependencies & Execution Order

### Phase Dependencies

- Setup (Phase 1) must complete before any foundation or story work begins.
- Foundational (Phase 2) must complete before User Story implementation starts.
- User Stories (Phase 3+) depend on Foundational but are otherwise independent.
- Polish (Phase 6) depends on completing the desired user stories.

### User Story Dependencies

- **US1**: Can start after Phase 2 and does not depend on US2 or US3.
- **US2**: Can start after Phase 2 and should integrate with US1 subscription data.
- **US3**: Can start after Phase 2 and validates the same request flow used by US1.

### Parallel Opportunities

- Setup tasks `T003` and `T004` can run in parallel.
- Foundational tasks `T005`, `T006`, `T007`, `T009`, and `T010` can largely run in parallel across backend and frontend files.
- Within US1, `T011` and `T014` can be parallelized because backend validation and frontend wiring are separate.
- US2 backend model/service work and frontend item display work can be parallelized where they do not depend on each other.
- US3 frontend error handling and backend validation improvements can be worked in parallel once the underlying API contract exists.

### Suggested MVP Scope

- Focus first on **US1** as the MVP deliverable.
- Validate the subscription add flow end to end before adding feed item retrieval (US2).
- Add robust invalid-input handling (US3) after the main add/view flow works.
