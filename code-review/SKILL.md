---
name: code-review
description: Review code changes for bugs, security issues, performance problems, and best practice violations.
---

# Code Review Skill

Review the current code changes and provide actionable feedback.

## Instructions

1. First, identify what has changed by running `git diff` (for unstaged changes) and `git diff --cached` (for staged changes). If no local changes exist, ask the user which branch or commit range to review.

2. For each changed file, analyze the diff and check for:
   - **Bugs**: Logic errors, off-by-one mistakes, null/undefined access, race conditions, unhandled edge cases.
   - **Security**: Injection vulnerabilities (SQL, XSS, command), hardcoded secrets, insecure defaults, missing input validation at system boundaries.
   - **Performance**: Unnecessary allocations, N+1 queries, missing indexes, expensive operations in hot paths, unbounded loops or data structures.
   - **Readability**: Unclear naming, overly complex logic, missing context for non-obvious decisions.
   - **Correctness**: Misuse of APIs, incorrect error handling, broken contracts with callers.

3. Output your review in this format:

### Summary
One-paragraph overview of the changes and their overall quality.

### Issues
For each issue found:
- **[severity]** `file:line` — description of the problem and a suggested fix.

Severity levels: `critical` (must fix), `warning` (should fix), `nit` (optional improvement).

### Verdict
State one of:
- **Approve** — no critical or warning issues.
- **Request changes** — critical or warning issues that should be addressed before merging.

4. Be concise. Do not praise code that is simply correct — focus on problems. If the code looks good, say so briefly and approve.
