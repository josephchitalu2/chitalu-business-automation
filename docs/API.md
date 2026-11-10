# API Proposal

These endpoints are design contracts, not implemented routes. Base prefix: /api/v1. Authentication and authorization are required before exposing business records.

| Method | Path | Intended behavior |
|---|---|---|
| POST | /customers | Create customer |
| GET | /customers | Search authorized customer records with pagination |
| POST | /credit-sales | Record authorized sale on credit |
| POST | /repayments | Record payment with an idempotency key |
| GET | /customers/{id}/statement | Calculate statement from ledger records |
| POST | /cash-movements | Record cash addition/removal with reason and actor |
| POST | /closings | Snapshot expected cash and record counted cash |
| GET | /closings/{id} | Retrieve discrepancy and supporting movements |
| POST | /expenses | Request an expense |
| POST | /expenses/{id}/decisions | Record authorized approval/rejection |
| POST | /expenses/{id}/payments | Record actual payment separately |
| GET | /tasks | View follow-up work |
| PATCH | /tasks/{id} | Change task status under allowed transitions |

Use explicit integer amount_ngwee fields and a currency field. Validate method, reference, customer and authorization relationships. Keep request identifiers for retry-safe financial writes. Paginate lists, reject invalid state transitions and return consistent errors without leaking private records.
