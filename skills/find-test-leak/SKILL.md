---
name: find-test-leak
description: Use when a Playwright test passes alone but fails in the full suite (order-dependent or state-leak failures). Finds the earlier test that pollutes state via binary search.
---

# Find leaking Playwright test

1. Run `npx playwright-leak-finder [same flags as the failing run, e.g. --project=chromium --config=...]`.
   - Don't pass `--workers`, `--reporter`, `--retries`, `--shard`, `--max-failures`, or bare spec-file filters; the tool rejects them. Use `--grep` or `--project` to narrow instead.
2. If it says the target passes alone, there is no leak. Stop and investigate the test itself.
3. Otherwise it prints "Suspects remaining: N". Run the exact same command again. Repeat until it names a single leaking test.
4. Open the leaking test and look for state it leaves behind (localStorage/cookies, DB rows, created employees or leave requests, logged-in sessions, shared fixtures). Fix it with proper cleanup or fixture teardown.
5. Re-run the full suite to confirm.

## Setup

Install in the project under test: `npm install --save-dev playwright-leak-finder` (requires `@playwright/test` >= 1.40).

To use this skill, copy this folder to `.claude/skills/find-test-leak/` in the project (or `~/.claude/skills/` for all projects).
