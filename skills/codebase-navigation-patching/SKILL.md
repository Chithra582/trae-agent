---
name: codebase-navigation-patching
description: "Explores codebases, parses ASTs, locates target symbols, and applies surgical diff patches."
---

# Codebase Navigation & Patching

## Overview
The `codebase-navigation-patching` skill enables precise exploration of complex repositories, syntax-aware symbol resolution, and surgical text replacements without disrupting surrounding indentation or style conventions.

## Navigation Protocol
1. **Symbol Search:** Use ripgrep or ast-grep patterns to locate definitions, usages, and import graphs.
2. **Context Window Slicing:** Read bounded line ranges around target functions rather than ingesting entire files.
3. **Patch Generation:** Construct exact find-and-replace hunks with unambiguous leading whitespace and line anchors.
4. **Post-Patch Verification:** Immediately inspect the modified file slice to verify syntactic correctness.
