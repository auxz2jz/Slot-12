# Rule Changelog

## 2026-09-27 — Diagnostics made mandatory and program-adaptive

Strengthened `DIAGNOSTICS_STANDARD.md` and `GUIDED_TESTING_STANDARD.md`.

Major additions:

- built-in diagnostics are now mandatory infrastructure for every program under the Master Instruction Library;
- each program must first be inspected to identify its actual controls, workflows, background jobs, automatic operations, important states, outputs, errors, and existing tests;
- examples such as Play/Pause/Seek/Tracking are explicitly examples only and must never create assumed controls;
- added a per-feature Diagnostic Coverage Map requirement;
- formalized diagnostic sessions with session IDs, timestamps, monotonic time, sequence counters, version/build identity, and optional test/step identity;
- strengthened JSONL/structured event logging, automatic/programmatic-action logging, high-frequency input throttling, request/result correlation, before/after state, input metadata, and performance timing;
- strengthened result verification so UI changes/progress indicators/process completion cannot alone prove success;
- strengthened error/crash preservation and machine-readable test-result export;
- every important new feature/background operation must extend diagnostics, PASS/FAIL criteria, guided testing, and export coverage;
- guided tests must be designed from the actual program's real features and internal success signals;
- added explicit diagnostic-driven error-correction workflow and final reconstruction requirement.

## 2026-09-26 — Cross-platform multi-agent collaboration standard added

Added `CROSS_PLATFORM_COLLABORATION_STANDARD.md`.

This standard supports projects where an Android/mobile ChatGPT worker and a Windows/PC agent develop separate implementations of the same product in one GitHub repository.

Key rules include:

- one shared product repository with clearly separated shared, Android, and Windows ownership zones;
- shared product vision, feature catalog, requirements, decisions, and common data/interface specifications;
- Android and Windows source, checkpoints, tests, diagnostics, candidate versions, and verified baselines remain separate;
- shared feature ideas propagate between platforms without requiring identical implementations;
- each platform evaluates whether shared features are applicable and records platform-specific status;
- one platform must not modify or overwrite the other platform's implementation without explicit authorization;
- shared files require latest-version reads, small edits, and conflict reconciliation;
- separate build/release artifacts and version numbers are allowed;
- either Android/mobile ChatGPT or a PC agent may initialize a new project;
- concurrent work safety and optional platform-specific branches are defined;
- cross-platform recovery must preserve independent verified baselines.

## 2026-09-25 — Recovery commands generalized and strengthened

Updated `CORE_DEVELOPMENT_RECOVERY_RULES.md` using recovery patterns from multiple existing projects.

Key changes:

- recovery now uses whatever checkpoint/handoff/source-manifest/verification files actually exist instead of assuming fixed filenames;
- last physically user-verified version takes priority over later unverified candidates;
- exact source artifact, commit/branch, ZIP/package identity, and hash/checksum are recovered when available and never invented when absent;
- `CONTINUE FROM CHECKPOINT` now requires only one concise review of newest evidence and then the single recorded next step;
- `STOP LOOP. CHECKPOINT ONLY.` now records supplied files/results, current version/status, test result, whether a new build actually started, and the single next action;
- added the anti-loop interrupt `YOU ARE REPEATING WORK. USE THE LAST CONFIRMED RESULT AND MOVE FORWARD ONCE.`;
- added generic full-recovery and checkpoint-before-build prompt forms.

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
