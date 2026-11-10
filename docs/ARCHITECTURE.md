# Application Architecture

## Decisions

The proposed application factory and grouping by credit, reconciliation and approval are a sound starting point. Business rules and data access must have explicit homes so that controllers do not become the entire application.

| Path | Responsibility |
|---|---|
| app/__init__.py | create_app factory, blueprints, CLI registration and error handling |
| app/config.py | validated configuration and environment selection |
| app/database.py | connections, transactions, foreign-key enforcement and request cleanup |
| app/models/credit.py | customer, credit sale, repayment and statement domain objects |
| app/models/transaction.py | cash movement, closing and discrepancy domain objects |
| app/models/workflow.py | expense approval and task domain objects |
| app/services/ | credit, reconciliation, approval and task business rules |
| app/repositories/ | parameterized SQL and mapping records to domain objects |
| app/routes/ | authenticated HTTP controllers; route-to-service calls |
| app/templates/ and app/static/ | rendered pages, styles and browser behavior |
| migrations/ | versioned database changes |
| tests/ | domain, repository, API authorization and workflow tests |
| run.py | local web-server entry point; not a console input loop |
| wsgi.py | production WSGI application export |

If a CLI is needed, implement it separately through Flask commands or a dedicated console entry point. Initialization and migrations must be deliberate operations, not schema mutation on every HTTP request.

## Data integrity

Use integer ngwee for stored amounts or a carefully defined Decimal boundary; never binary floating point for money. A statement balance is computed from credit sales, repayments and explicit corrections, not an independently editable number.

Record actor, timestamp, currency, reason and reference. Represent paid and approved expenses as distinct states. Record financial corrections through reversals or adjustment entries with an audit trail. Approval does not itself mean cash left the drawer.

Repayment recording, balance checks and ledger writes must occur in one database transaction. Unique idempotency keys and constraints prevent duplicate recording when a client retries a request. Enforce payment limits and the policy for overpayments explicitly.

Expected cash = opening float + recorded cash inflows - recorded cash outflows. Card and mobile-money sales do not increase physical drawer cash. Settling an old credit account affects the drawer only if repayment was cash.

## Security and storage

Enforce owner/manager/cashier permissions server-side, protect cookie-authenticated writes against CSRF, hash login passwords with an appropriate supported password-hashing implementation, validate inputs and use parameterized queries. Do not publish customer details or production data in Git.

SQLite is suitable for the initial single-instance installation with tested transaction behavior and backups. Managed PostgreSQL is a candidate for shared hosted operation. SQLite files require persistent storage and must not be treated as durable storage on a temporary serverless filesystem.

## First complete workflow

Create customer -> authorize and record credit sale -> record partial repayment -> calculate statement -> surface due follow-up -> record payment method and update closing where applicable. Test retries, rejected inputs, rollback, unauthorized access and restoration of a backup.
