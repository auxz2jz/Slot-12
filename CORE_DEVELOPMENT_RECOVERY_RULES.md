# Core Development and Recovery Rules

These rules apply throughout the lifetime of every software project unless the user explicitly changes them.

## 1. Permanent project memory

Do not rely only on chat history.

Each project must maintain permanent project documentation containing at least:

- current project status
- current version/build
- last user-verified working version
- latest unverified candidate
- current task
- implementation plan
- roadmap
- known bugs and limitations
- failed approaches
- important architecture/design decisions
- test results
- diagnostic findings
- files/results already received
- exact next action

Another developer or another ChatGPT conversation must be able to continue from saved project files.

## 2. Verified baseline

The most recent version the USER personally tested and confirmed works is the **LAST VERIFIED BASELINE**.

Never overwrite, destroy, silently replace, or lose its exact source.

A version created by the developer/AI is a **CANDIDATE** until the user tests it.

Recommended statuses:

- PLANNED
- IN PROGRESS
- CANDIDATE
- PARTIAL
- VERIFIED
- FAILED
- BLOCKED
- SUPERSEDED
- DEFERRED
- DONE

Do not call code VERIFIED because it compiled. Do not call a feature DONE merely because code exists.

## 3. Startup procedure

Before substantial changes:

1. Read this master instruction library.
2. Read project memory/checkpoints.
3. Read the roadmap.
4. Read testing/diagnostic instructions.
5. Identify the last verified baseline.
6. Identify the latest legitimate candidate.
7. Inspect relevant source.
8. Review latest user test evidence.
9. Review known bugs and failed approaches.
10. Record the new task.
11. Write the implementation plan.
12. Identify files expected to change.
13. Preserve/checkpoint the known-good state before risky work.

Do not begin by randomly editing code.

## 4. Golden workflow

Use:

**READ → PLAN → RECORD PLAN → CHECKPOINT → MODIFY → BUILD → TEST → ANALYZE → RECORD RESULT → CHECKPOINT → CONTINUE**

Whenever practical, make one logical change at a time, build it, test it, record the result, and understand the result before adding unrelated changes.

## 5. Checkpoints

Create recoverable checkpoints:

- whenever the user verifies a version
- before a major feature
- before replacing working code
- before major refactoring
- before architecture changes
- before changing many important files
- after finding an important root cause
- after creating a testable candidate
- before ending a work session

Checkpoint contents should include:

- version/build
- candidate/verified status
- exact source/artifact
- commit/hash when available
- what changed
- what works
- what remains unverified
- known failures
- test results
- diagnostic findings
- exact next action

## 6. Anti-loop rule

Never repeatedly try substantially the same failed solution without new evidence.

If essentially the same approach fails twice:

**STOP THAT APPROACH.**

Record:

- what was attempted
- what happened
- exact error or bad behavior
- relevant logs/diagnostics
- what was learned
- which assumption may be wrong
- what evidence is needed next

Then choose a meaningfully different approach.

Do not:

- repeatedly apply the same patch
- repeatedly rebuild unchanged broken code
- bounce between two failed solutions
- randomly change unrelated working code
- reread the same material without a reason
- guess when direct evidence is available

## 7. Three-failure escalation

If three meaningfully different attempts at the same problem fail:

1. Stop implementation on that problem.
2. Record all three failed approaches.
3. Re-read relevant source.
4. Compare with the last verified baseline.
5. Re-check architecture and assumptions.
6. Inspect diagnostics/logs/test evidence.
7. Write a new diagnostic plan.
8. Continue only when the next attempt is based on new evidence.

## 8. Evidence-first debugging

When the user supplies compiler output, logs, diagnostic packages, screenshots, crash reports, test reports, or generated files, inspect that evidence before modifying code.

Identify the **first real failure** rather than merely the last visible symptom.

Make the smallest targeted correction supported by evidence.

## 9. Protect working behavior

Before replacing working code:

1. Explain why replacement is needed.
2. Record what the current implementation does.
3. Preserve a checkpoint.
4. Change only what is necessary.
5. Retest directly affected verified behavior.

Do not fix Feature B by unnecessarily rewriting Feature A.

## 10. Versioning

Keep testable checkpoints versioned.

Examples:

- v0.1.0 = meaningful initial candidate
- v0.2.0 = meaningful new feature
- v0.2.1 = bug-fix candidate

Do not increment for every tiny internal edit.

Keep the previous verified version until the new candidate passes user testing.

## 11. Truthfulness about artifacts and progress

Never claim a file, APK, EXE, ZIP, build, commit, test, upload, or source update exists unless it actually exists.

A discussed future version is not a completed version.

A successful build is not proof the feature works on the user's device.

## 12. End-of-work procedure

Before ending a development session:

1. Save changed source.
2. Update the current task.
3. Record completed work.
4. Record failures/errors.
5. Record diagnostics/root causes.
6. Update feature statuses.
7. Record the current candidate.
8. Update last verified version only if the user actually verified it.
9. Update the roadmap.
10. Record failed approaches.
11. Record files/results already received.
12. Record the exact next action.
13. Save/checkpoint the project.

## 13. Recovery procedure

If development becomes confused, repetitive, or starts looping:

**STOP CHANGING CODE.**

Then:

1. Read project memory.
2. Read latest checkpoint.
3. Identify last verified baseline.
4. Identify latest legitimate candidate.
5. Read current task.
6. Review errors and failed approaches.
7. Inspect current source.
8. Compare with verified source.
9. Inspect diagnostic evidence.
10. Identify exact unfinished/failing step.
11. Write a new plan.
12. Resume only from that known state.

## 14. Recovery commands

If the user says:

**STOP LOOP. RECOVER LAST VERIFIED CHECKPOINT. NO NEW WORK.**

Stop development, recover the exact last verified version/source/state, and report it before doing anything else.

If the user says:

**STOP LOOP. CHECKPOINT ONLY.**

Stop development and analysis, save the exact current state and single next action, and do not continue until instructed.

If the user says:

**CONTINUE FROM CHECKPOINT.**

Read the latest checkpoint as the source of truth and perform the recorded next action without repeating completed analysis.

## Final rule

Protect working code, preserve recoverable checkpoints, test small changes, use evidence instead of guesses, accurately record failures, and never allow important project knowledge to exist only inside the current chat.
