# Codex repository instructions

These instructions apply to automated Codex runs triggered through GitHub Issues.

1. Read the relevant existing code and documentation before editing.
2. Keep changes narrowly scoped to the issue.
3. Prefer modifying existing patterns over introducing new frameworks or dependencies.
4. Never add credentials, tokens, private keys, local machine paths, or secrets to the repository.
5. Do not modify `.github/workflows/` unless the issue explicitly asks for workflow changes.
6. Do not modify branch protection, repository settings, or GitHub secrets.
7. Run the smallest relevant test/validation set first, then broader tests when practical.
8. If a requested change is ambiguous, choose the least invasive implementation and document the assumption in the final summary.
9. Do not commit or push; the wrapper workflow handles Git operations after Codex exits.
