# Chitalu Business Automation

**Chitalu Software Solutions**

A planned operations platform for customer credit, repayment reconciliation, cash closing, expenses, approvals and follow-up tasks. The repository begins with the agreed architecture and API design. The application is not yet implemented or deployed; the first delivery milestone is an executable credit-to-payment workflow with tests.

## Product scope

- Customer credit sales, partial repayments and statements.
- Daily cash closing with opening float, cash movements and discrepancies.
- Expense requests, approvals and payment tracking.
- Scheduled follow-up tasks and owner reporting.
- Role-based access, correction history and restore-tested backups.

## Architecture

Flask application factory with separate domain models, services, repositories and HTTP routes. SQLite is a candidate for a single-instance local installation; a hosted multi-instance service should use an appropriate managed database. Production data must not live in a serverless deployment's temporary filesystem.

Read the [architecture](docs/ARCHITECTURE.md), [API proposal](docs/API.md) and [delivery plan](docs/DELIVERY_PLAN.md). The [first delivery walkthrough](docs/FIRST_SLICE.md) specifies the results to verify for a credit sale and partial repayments.

## Status

Design baseline only. There are no running endpoints, financial records or production credentials in this repository. No passing application tests are claimed yet. The aim is a complete, tested release; roadmap milestones organize implementation rather than substitute for working features.

## Local checkout

Clone the repository using the complete remote URL:

```bash
git clone https://github.com/josephchitalu2/chitalu-business-automation.git
cd chitalu-business-automation
```

Run `python3 scripts/check_docs.py` from this directory to check local documentation links.

## Maintenance

[Verification](docs/TESTING.md) · [Contributing](CONTRIBUTING.md) · [Security reporting](SECURITY.md)
