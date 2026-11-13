# Verification

Requires Python 3.10+; no third-party dependencies for these checks. Run from the repository root:

```bash
python3 scripts/check_docs.py
```

The checker verifies relative Markdown links. The business endpoints are still proposals, so there are no application tests yet. Use the [first delivery walkthrough](FIRST_SLICE.md) to build tests once the credit and repayment workflow exists.

GitHub Actions runs the checker on pushes to `main` and on pull requests. Record actual browser, environment, service-integration and recovery tests when those features exist. Do not interpret passing link checks as production or security verification.
