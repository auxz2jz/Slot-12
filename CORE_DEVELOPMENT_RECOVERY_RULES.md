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

1. Read the project's designated checkpoint/handoff/source-of-truth files that actually exist.
2. If no single designated checkpoint file exists, inspect the repository for the available equivalents, such as project memory, project status, "start here" handoff, verification history, roadmap, source manifest, build/release notes, test-result manifests, or similar files.
3. Do not fail recovery merely because a particular expected filename is absent. Use the project's actual available recovery files.
4. Identify the **last version physically tested and confirmed by the user**.
5. Identify the exact verified source artifact, commit, branch, ZIP/package name, and hash/checksum when those values are available.
6. If a requested hash or artifact identity was never recorded, state that it is unavailable; never invent it.
7. Identify the latest legitimate unverified candidate separately.
8. Ignore later unverified/confused work as a baseline unless the user explicitly verified or approved it.
9. Read the current task and exact recorded next action.
10. Review errors and failed approaches.
11. Inspect current source only as needed to reconcile it with the verified baseline and newest evidence.
12. Inspect diagnostic evidence.
13. Identify the exact unfinished/failing step.
14. Write a new plan only if the checkpoint does not already contain a valid next step.
15. Resume only from that known state.

### Recovery source-of-truth rule

During recovery, evidence priority is:

1. User's explicit physical test/verification.
2. Exact verified source artifact/commit/hash recorded with that test.
3. Latest valid checkpoint/handoff state.
4. Newest diagnostic/test evidence.
5. Unverified candidate source.
6. Older chat discussion or assumptions.

A later-created version does not supersede a physically verified baseline merely because it has a higher version number.

## 14. Recovery commands

These commands are intentionally short. They may be used in any project regardless of the exact checkpoint/handoff filenames.

### STOP LOOP. RECOVER LAST VERIFIED CHECKPOINT. NO NEW WORK.

Immediately:

1. Stop coding, building, editing, and experimentation.
2. Read the available project checkpoint/handoff/source-manifest/verification files.
3. Identify the last version the user physically confirmed as working.
4. Identify its exact source/artifact, commit/branch, and hash/checksum when available.
5. Ignore later unverified work as a recovery baseline.
6. Report the recovered state, known problems, and exact next recorded action.
7. Do not create or modify code until the user instructs you to continue.

### STOP LOOP. CHECKPOINT ONLY.

Immediately:

1. Stop all current analysis, coding, and build work.
2. Save the exact current project state to the project's available checkpoint/handoff mechanism.
3. Record which user-supplied files/results are already available.
4. Record the current version and whether it is VERIFIED, CANDIDATE, PARTIAL, FAILED, or another accurate status.
5. Record the latest test result.
6. Record whether a new version/build has actually started or whether it was only discussed/planned.
7. Record the current source/artifact identity and hash when available.
8. Record the single next action.
9. Do not continue until instructed.

### CONTINUE FROM CHECKPOINT.

Immediately:

1. Use the latest valid checkpoint/handoff state as the source of truth.
2. Confirm the last physically verified baseline.
3. Review only the newest relevant results once.
4. Do not repeat analysis already recorded as complete.
5. Perform the single recorded next build/development step.
6. If that next step is no longer valid because of new evidence, explain the specific conflict and choose the smallest evidence-based correction.

### YOU ARE REPEATING WORK. USE THE LAST CONFIRMED RESULT AND MOVE FORWARD ONCE.

Treat this as an anti-loop interrupt.

Immediately:

1. Stop rereading/re-explaining/rechecking the same material.
2. Use the latest confirmed result already established.
3. Do not repeat the same analysis unless genuinely new evidence requires it.
4. Perform the next evidence-based action exactly once.
5. If no valid next action is known, checkpoint the state and identify the single missing piece of evidence rather than continuing to loop.

### Generic full recovery prompt

The following longer form may be used when a project is badly confused:

**STOP. You are stuck in a loop. Do not create or modify any more code. Read the project's available checkpoint, handoff, verification-history, roadmap/status, source-manifest, build/release-note, and test-result files that actually exist. Identify the last version I physically confirmed as working and the exact source artifact/commit/hash when available. Ignore anything created after that unless I explicitly verified it. Tell me the recovered state before doing any more work.**

### Generic checkpoint-before-build prompt

For risky or replacement builds, the user may say:

**Checkpoint first. Before making changes, confirm the last physically verified version, exact source/artifact, hash/checksum when available, and current handoff/checkpoint state. Build the next candidate only from that known source or an explicitly approved successor. Do not overwrite the verified baseline.**

## Final rule

Protect working code, preserve recoverable checkpoints, test small changes, use evidence instead of guesses, accurately record failures, and never allow important project knowledge to exist only inside the current chat.
