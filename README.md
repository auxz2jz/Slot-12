# Master Instruction Library

This repository is the reusable source of truth for development rules that should apply across software projects.

## Purpose

Use this repository to store project-wide rules that should be reused in future projects, including:

- anti-loop and recovery behavior
- project-memory and checkpoint requirements
- verified-baseline protection
- conservative error correction
- built-in diagnostics and action tracing
- button/action and error logging
- crash/stall monitoring
- guided "Test This Version" workflows
- automatic and manual PASS/FAIL verification
- diagnostic export requirements

## How a project should use this library

Before substantial work begins on a project:

1. Read `INSTRUCTION_INDEX.md`.
2. Read every file marked **MANDATORY** that applies to the project.
3. Then read the individual project's own memory/checkpoint, roadmap, diagnostics, and test documents.
4. Project-specific instructions supplement this library.
5. The user's latest explicit instruction takes priority if a direct conflict exists.

## Rule-library principle

This repository contains reusable rules, not the current state of any one application.

Individual projects should keep their own:

- current version
- last verified baseline
- candidate version
- current task
- roadmap
- bugs
- test results
- exact next action

## Adding future rules

When the user identifies a rule they want across projects, add it to the appropriate instruction file and record the addition in `RULE_CHANGELOG.md`.

Do not silently delete or weaken an existing rule when adding a new one. If rules conflict, document the conflict and preserve the user's latest explicit decision.

## Initial status

Initialized as the Master Instruction Library in Slot-12. The repository may be renamed later without changing its purpose.
