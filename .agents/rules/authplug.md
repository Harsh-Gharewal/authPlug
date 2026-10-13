---
trigger: always_on
---

# Auth-Plug project rules

Project: Auth-Plug, an open-source npm package (authentication framework for Node.js) with a CLI and an optional AI security layer. The full requirements are in docs/FRD.md. Always read it before planning.

## Engineering rules
- Language: TypeScript, strict mode, Node.js 20+. No `any`. Public APIs have JSDoc.
- Package is dual-published (ESM + CJS) with bundled type definitions.
- Every feature ships with Vitest tests in the same change. Never mark work done without running the tests and reporting the results.
- Storage is accessed ONLY through the adapter interface in src/adapters. Never import a database driver from core code.
- Notifications (email/SMS) go ONLY through the notifier interface in src/notifiers.
- Keep dependencies minimal. Before adding a dependency, state why and prefer well-maintained, widely used packages.

## Security rules (non-negotiable)
- Passwords: Argon2id by default, bcrypt optional. Never log, return, or store plaintext passwords or raw tokens.
- Verification, reset, OTP, refresh, and API-key secrets are stored HASHED, are single-use where applicable, and expire.
- JWT: pin allowed algorithms explicitly (reject "none"), always verify exp/iss/aud, support key rotation via `kid`.
- Use constant-time comparison for secrets. Error messages must not reveal whether an account exists.
- Never hard-code secrets, keys, or credentials. Read them from config/env. Never create or edit .env files.
- Use parameterized queries only.

## Workflow rules
- Always produce an implementation plan and task list first and WAIT for my approval.
- Only touch files inside the folders named in the task. If something outside scope needs changing, stop and ask.
- Do not add features not listed in the task. Stub future items with clear TODO comments and a typed interface.
- Never run `npm publish`, `git push --force`, or commands that delete files outside the project.
- End every task with a summary: what was built, test results, what was NOT done, and any decisions I need to make.