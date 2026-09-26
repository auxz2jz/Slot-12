# Rule Changelog

## 2026-09-25 — Rule intake workflow added

Added `RULE_INTAKE_WORKFLOW.md`.

This establishes the Master Instruction Library as an actively maintained rule system rather than a simple append-only prompt. New user-supplied global rules should now be reviewed against existing instructions and either merged, clarified, split into a subsection, or added as a new category. Duplicate and conflicting rules should be resolved deliberately and documented.

This file records global instruction-library changes.

## 2026-09-25 — Repository renamed

The canonical repository was renamed from `auxz2jz/Slot-12` to:

`auxz2jz/master-instruction-library`

No rule behavior changed. This is now the permanent repository identity to reference from future projects.

## 2026-09-25 — Initial library

Repository initialized in Slot-12 as the future **Master Instruction Library**.

Initial rule sets added:

- project memory and checkpoint preservation
- verified baseline vs candidate distinction
- small-change development workflow
- anti-loop two-failure stop rule
- three-failure escalation rule
- evidence-first debugging
- protection of working features
- recovery commands
- persistent built-in diagnostic event logging
- semantic button/user-action tracing
- optional raw touch tracing
- before/after setting logging
- correlation/run/test IDs
- persistent rolling Action Trace
- error and stack-trace logging
- global crash preservation
- stall/watchdog monitoring
- result/output validation
- structured operation reports
- pipeline dependency invalidation
- guided "Test This Version" workflow
- persistent test progress
- automatic PASS/FAIL verification
- human visual confirmation
- manual failure reporting
- diagnostic/test report export
- privacy/redaction requirements

Future additions should be appended here with date, affected file, and a short explanation.
