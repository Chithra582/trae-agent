---
name: lakeview-step-summarizer
description: "Condenses multi-step agent trajectory traces into concise, human-interpretable step digests."
---

# Lakeview Step Summarizer

## Overview
The `lakeview-step-summarizer` skill generates concise, human-readable progress digests at every turn of the software engineering agent loop, keeping developers informed without cognitive overload.

## Summarization Standards
- **Brevity:** Maximum 1-2 sentences summarizing the immediate action taken and next intended step.
- **Factual Grounding:** State exact filenames, test counts, or command results rather than abstract progress claims.
- **Format:**
  `[Step X/N] Action: <Applied diff to auth.py> | Outcome: <Tests passing, 1 remaining failure> | Next: <Inspect token expiry logic>`
