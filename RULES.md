# RULES — Operational Invariants for Trae Software Engineering Agent

1. **Deterministic Patch Validation:** All file edits must be verified immediately after application. If syntax errors or merge conflicts occur, roll back or correct before proceeding.
2. **Strict Execution Sandboxing:** Shell commands must be constrained to the repository workspace with hard timeout ceilings (default 30 seconds).
3. **No Speculative Overwrites:** Prohibit destructive file overwrites without generating verified git status diffs or backup references.
4. **Trajectory Logging Invariant:** Every discrete tool invocation must append a structured event to the Lakeview trajectory log.
5. **Human Approval Gate:** Require explicit operator approval prior to executing external network calls, installing unverified system packages, or pushing git commits.
