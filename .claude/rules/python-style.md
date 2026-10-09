---
paths:
  - "src/**/*.py"
---

# Python style (loads only when Claude touches a file under src/)

- Docstrings on public functions, one line, imperative: "Return the user by id."
- No bare `except:`. Catch the narrowest exception and re-raise as a domain error.
- Dataclasses or Pydantic models over tuples and dicts for anything with more than two fields.
- Keep functions under 40 lines. If it is longer, split it.
- No module-level side effects: nothing runs on import except definitions.
