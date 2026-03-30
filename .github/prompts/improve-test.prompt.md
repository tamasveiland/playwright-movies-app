---
description: improves the tests
agent: 🎭 polyglot-test-generator.agent
---

Improve the specified test. Align with the following guidelines:

We are testing a large customer-facing web portal with a React frontend and REST APIs.

Objective:
Refactor this Playwright test to reduce flakiness and make failures easier to diagnose in CI.

Constraints:
- Use Playwright with TypeScript
- Avoid hard sleeps or fixed timeouts
- Prefer role-based or data-test selectors
- Keep assertions explicit and readable for QA reviewers

Risk focus:
- Timing issues due to async rendering
- Elements appearing but not being interactive
- Error messages failing silently

Output expectations:
- Clean, readable test structure
- Clear assertion messages
- Minimal but meaningful comments
