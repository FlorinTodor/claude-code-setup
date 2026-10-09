---
name: feedback-testing
description: Never mock the repository layer; iterate on one test file, not the whole suite
type: feedback
---

Mock only the network boundary. The repository and the database are real in tests (the `db` fixture spins up Postgres in Docker).

**Why:** a mocked repository hid a double-billing bug for two months.

**How to apply:** when writing or fixing tests, use the fixtures in `tests/conftest.py`. While iterating run `pytest tests/test_<file>.py`, not `make test`.
