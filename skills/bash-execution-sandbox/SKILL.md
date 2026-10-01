---
name: bash-execution-sandbox
description: "Executes shell commands, test runners, and build systems inside bounded execution environments."
---

# Bash Execution Sandbox

## Overview
The `bash-execution-sandbox` skill orchestrates command-line tasks, running test suites, build compilers, and package managers with strict resource quotas and safety bounds.

## Operational Boundaries
- **Working Directory:** All commands must run strictly within the workspace directory tree.
- **Timeout Caps:** Impose default 30-second timeouts on shell invocations to prevent hangs from interactive prompts.
- **Output Truncation:** Limit command stdout and stderr to the first and last 50 lines to prevent context window overflow.
