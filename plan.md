Plan

Analyze existing article module (entity, service, controller, DTOs, frontend forms) to ensure consistency with current architecture and patterns.

Extend data model to support co-authors using a many-to-many relationship between articles and users.

Create database migration adding join table article_co_authors.

Update Article entity and DTOs to support co-author IDs with validation.

Update article create and update endpoints to persist co-authors.

Implement authorization logic at service layer ensuring only author or co-authors can edit articles.

Add endpoint to fetch users for dropdown selection.

Update Create Article UI to include multi-select dropdown for co-authors.

Update Edit Article UI to enforce permission validation.

Implement article locking system:

Create table article_locks with fields articleId, lockedBy, lockedAt, expiresAt.

Add endpoint to acquire lock when editor opens article.

Add endpoint to release lock on save or navigation away.

Validate lock ownership on update requests.

Expire locks automatically using timestamp comparison.

Implement backend validation middleware/guard that prevents edits without lock ownership.

Add frontend handling for lock conflicts and lock loss scenarios.

Manually run acceptance tests and capture screenshots.

Confirm existing pages remain unchanged and display only original author.

Submit solution.

Decisions
Decision 1 — Many-to-Many Join Table for Co-Authors

Alternative: Store comma-separated emails.

Alternative: Store JSON array column.

Rationale: Join table preserves referential integrity, enables efficient queries, enforces valid users, and aligns with relational database normalization principles.

Decision 2 — Server-Side Locking with Expiration Timestamp

Alternative: Frontend-only locking.

Alternative: WebSocket presence-based locking.

Rationale: Server-side locking guarantees consistency across distributed instances and prevents race conditions. Timestamp expiration prevents stale locks without requiring persistent connections or additional infrastructure.

Decision 3 — Lock Validation via Backend Guard

Alternative: Validate lock only in controller logic.

Alternative: Validate only in frontend.

Rationale: A guard ensures centralized and reusable enforcement of locking rules across all write operations, reducing duplication and preventing accidental bypass.

Notes

No AWS infrastructure changes are required since locking state is persisted in the shared database, making it compatible with horizontally scaled environments.

Lock state stored in database ensures consistency across multiple application instances.

Co-authors functionality is isolated from existing article display logic to avoid regressions.

Feature is implemented strictly within scope to avoid unintended side effects.