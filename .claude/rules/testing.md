---
paths:
  - "tests/**/*.py"
---

# Testing (loads only when Claude touches a file under tests/)

- One behaviour per test. Name it `test_<unit>_<scenario>_<expected>`.
- Arrange, act, assert, separated by a blank line. No comments saying "arrange".
- Use the fixtures in `tests/conftest.py`; do not build a database session by hand.
- Mock only the network boundary (external HTTP, email). Never mock the repository or the database.
- A bug fix comes with a test that fails before the fix and passes after.
