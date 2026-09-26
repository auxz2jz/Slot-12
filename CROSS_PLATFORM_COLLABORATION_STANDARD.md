# Cross-Platform Multi-Agent Collaboration Standard

This standard applies when one software project has multiple platform implementations maintained by different ChatGPT chats, desktop/PC agents, or developers.

Typical example:

- Android ChatGPT/chat develops the Android application.
- PC/Desktop agent develops the Windows/PC application.
- Both use the same GitHub repository as the project's durable source of truth.
- Both should implement the same product ideas when technically appropriate.
- Neither should overwrite, reorganize, or accidentally modify the other platform's implementation.

The goal is:

**ONE PRODUCT / SHARED INTENT / SEPARATE PLATFORM IMPLEMENTATIONS**

---

## 1. One project repository, separate ownership zones

A cross-platform project may use one GitHub repository, but the repository must be divided into clearly separated areas.

Recommended structure:

```text
/
  README.md

  shared/
    PROJECT_VISION.md
    FEATURE_CATALOG.md
    SHARED_REQUIREMENTS.md
    SHARED_DECISIONS.md
    DATA_FORMATS_AND_INTERFACES.md        # only when needed

  android/
    PROJECT_MEMORY.md
    ANDROID_STATUS.md
    ANDROID_ROADMAP.md                   # platform-specific work only
    ANDROID_TESTING.md
    ANDROID_DIAGNOSTICS.md
    source/ or normal Android project files

  windows/
    PROJECT_MEMORY.md
    WINDOWS_STATUS.md
    WINDOWS_ROADMAP.md                   # platform-specific work only
    WINDOWS_TESTING.md
    WINDOWS_DIAGNOSTICS.md
    source/ or normal Windows project files
```

The exact filenames/folders may differ for an existing project.

Do not force a repository to use these exact names if equivalent files already exist.

What matters is that shared and platform-owned information is clearly separated.

---

## 2. Platform ownership

The Android worker owns the Android implementation area.

The Windows/PC worker owns the Windows implementation area.

Unless explicitly instructed otherwise:

### Android worker may modify

- Android source code
- Android build files
- Android-specific assets/resources
- Android project memory/status
- Android tests
- Android diagnostics
- Android candidate/version records

### Windows/PC worker may modify

- Windows/PC source code
- Windows build/project files
- Windows-specific assets/resources
- Windows project memory/status
- Windows tests
- Windows diagnostics
- Windows candidate/version records

### Neither platform worker may casually modify

- the other platform's source
- the other platform's project memory
- the other platform's verified baseline
- the other platform's build configuration
- the other platform's diagnostics/tests
- the other platform's release artifacts

If a cross-platform issue appears to require changes to the other platform, record the finding in shared coordination documentation and leave the other platform implementation to its owner unless the user explicitly authorizes cross-platform modification.

---

## 3. Shared coordination area

The shared area contains information that both platform implementations need.

Keep shared files platform-neutral whenever possible.

Recommended shared information:

### PROJECT_VISION.md

Contains:

- product purpose
- intended user experience
- major product goals
- high-level scope
- shared terminology

It should not contain platform-specific build instructions.

### FEATURE_CATALOG.md

This is the shared feature/idea list.

Every cross-platform feature should receive a stable Feature ID when practical.

Example:

```text
F-014 — Multi-file merge
Intent:
Allow the user to select multiple files and merge them in order.

Shared behavior:
- user can reorder inputs
- resulting output should be validated
- failure should preserve diagnostics

Platform notes:
Android and Windows may implement the UI differently.
```

If the Android chat invents a useful product feature, it should add the platform-neutral idea here so the Windows agent sees it.

If the Windows agent invents a useful product feature, it should do the same.

Adding an idea to the shared catalog does NOT automatically mean both platforms must implement it identically.

Each platform evaluates whether the feature is:

- APPLICABLE
- PLANNED
- IMPLEMENTED
- VERIFIED
- NOT APPLICABLE
- BLOCKED BY PLATFORM
- DEFERRED

Platform-specific status belongs primarily in platform-owned status/roadmap files.

### SHARED_REQUIREMENTS.md

Contains behavior that should remain consistent where feasible.

Examples:

- supported file semantics
- naming conventions
- project/save-file behavior
- core feature definitions
- privacy rules
- diagnostic behavior
- interoperability expectations

### SHARED_DECISIONS.md

Record important cross-platform decisions such as:

- feature renamed globally
- file format changed
- common behavior changed
- compatibility requirement adopted
- one platform cannot support a feature and why

This should be concise and durable.

### DATA_FORMATS_AND_INTERFACES.md

Create/use this only when the platforms exchange or consume common data.

Examples:

- project-file schema
- JSON format
- export format
- network/API contract
- shared database schema
- file naming/layout
- interchange metadata

Do not create this file if the project has no cross-platform data contract.

---

## 4. Shared roadmap concept vs platform roadmaps

Do not use one giant platform-specific roadmap for both implementations.

Use two levels:

### Shared feature catalog / product roadmap

Answers:

**What should the product be able to do?**

This is common across platforms.

### Platform roadmap

Answers:

**How and when will this platform implement it?**

Android and Windows may have different:

- technical approaches
- libraries
- UI
- development order
- limitations
- test procedures
- release versions

A shared feature can be implemented on Android first and Windows later, or vice versa.

One platform must not falsely mark the other platform's feature as DONE.

---

## 5. Feature propagation rule

When either platform discovers or adds a feature that is potentially useful to both platforms:

1. Record the platform-neutral feature idea in the shared feature catalog.
2. Give it a stable Feature ID when practical.
3. Record the originating platform if useful.
4. The other platform must notice the feature during its next normal startup/review.
5. The other platform evaluates whether it can and should implement the feature.
6. If applicable, add it to that platform's own roadmap.
7. If not applicable, record why rather than silently ignoring it.

Do not copy platform-specific implementation code merely because the feature idea is shared.

Share **intent and behavior**, not necessarily implementation.

---

## 6. Shared file change discipline

Because both workers may update shared documentation, shared files require extra care.

Before modifying any shared file:

1. Fetch/read the latest repository version.
2. Check whether another platform changed it since the last read.
3. Make the smallest necessary edit.
4. Preserve unrelated content.
5. Do not rewrite the whole file just to add one feature.
6. Commit/save the update promptly.
7. If a write conflict occurs, stop and reconcile both versions. Never force-overwrite the other worker's changes.

Shared documents should change less frequently than platform-specific implementation files.

---

## 7. Platform source isolation

Platform code must live in clearly distinct directories or otherwise clearly distinct repository areas.

Recommended:

- `android/`
- `windows/`

Do not place unrelated Android and Windows source files together in one ambiguous source tree.

Platform-specific generated files, build outputs, dependencies, and temporary files should also remain separated.

The Android build system must not depend on Windows-only project files unless intentionally sharing platform-neutral assets.

The Windows build must not depend on Android-only build files unless intentionally sharing platform-neutral assets.

---

## 8. Shared code is optional, not assumed

Do not assume code should be shared merely because the product is shared.

Only create a shared-code module when:

- the programming languages/toolchains permit it,
- the code is genuinely platform-neutral,
- sharing it reduces maintenance risk,
- both platform workers understand and agree on the dependency.

Otherwise share specifications, formats, algorithms, test vectors, or behavior rather than source code.

---

## 9. Separate verified baselines

Each platform has its own verified baseline.

Maintain separately:

- ANDROID LAST VERIFIED BASELINE
- WINDOWS LAST VERIFIED BASELINE

An Android verification never verifies the Windows build.

A Windows verification never verifies the Android build.

Each platform independently tracks:

- verified version
- candidate version
- exact source
- commit/hash when available
- build artifact
- test result

---

## 10. Separate version numbers when necessary

Android and Windows versions do not need to stay numerically identical.

For example:

- Android v0.8.0
- Windows v0.5.2

Shared Feature IDs provide cross-platform continuity even when release numbering differs.

Do not artificially increment one platform merely to match the other's version.

---

## 11. Separate checkpoints and handoffs

Each platform must maintain its own recovery state.

Android recovery documents should identify Android source/checkpoints.

Windows recovery documents should identify Windows source/checkpoints.

A shared project checkpoint may summarize overall product status, but it must never replace the platform-specific verified-baseline records.

When recovering Android, do not roll back Windows.

When recovering Windows, do not roll back Android.

---

## 12. Cross-platform startup procedure

Whenever an Android chat, normal ChatGPT chat, or PC agent begins/resumes work:

1. Read the Master Instruction Library.
2. Read this Cross-Platform Multi-Agent Collaboration Standard.
3. Read the project's shared coordination files.
4. Determine which platform this worker owns for the current task.
5. Read that platform's project memory/checkpoint/status/roadmap/testing files.
6. Identify that platform's last verified baseline.
7. Review newly added shared features/decisions since the previous work session.
8. Add applicable shared features to the platform roadmap if not already represented.
9. Do not modify the other platform's source.
10. Continue using the normal checkpoint/development workflow.

---

## 13. New-project initialization

A project may be started by either:

- Android/mobile ChatGPT
- normal ChatGPT
- Windows/PC agent

The worker that starts the project should initialize:

1. common repository structure;
2. shared product vision;
3. shared feature catalog;
4. shared requirements as needed;
5. its own platform area;
6. an empty or clearly marked placeholder/platform handoff area for the other implementation if useful.

Do not invent implementation status for the platform that has not started yet.

Example:

```text
Windows status: NOT STARTED
Android status: CANDIDATE v0.1.0
```

When the second platform begins, it reads the shared information and creates its own implementation plan.

---

## 14. Handoff between platforms

When one platform adds something the other should know:

Record only the reusable information in shared documentation.

Examples worth sharing:

- new product feature
- changed feature behavior
- file-format decision
- user workflow decision
- shared naming convention
- algorithmic discovery that is platform-neutral
- test case that should exist on both platforms
- security/privacy requirement
- interoperability bug
- common data schema change

Do not copy routine platform-specific compiler errors, UI implementation details, dependency versions, or source changes into shared documentation unless they materially affect the other platform.

---

## 15. Cross-platform test parity

When a shared feature exists on both platforms, each platform should have its own test for the same intended behavior.

The UI steps may differ.

Example:

Shared requirement:
F-014 — merge multiple files in chosen order.

Android test:
Use Android file picker and verify merged output.

Windows test:
Use Windows file picker and verify merged output.

Both tests validate the same product behavior while remaining platform-specific.

---

## 16. Cross-platform diagnostics

Use the Master Diagnostics Standard on both platforms.

The event formats do not need to be byte-for-byte identical, but shared concepts should remain recognizable:

- USER_ACTION
- OPERATION_START
- STATE/PROGRESS
- RESULT
- ERROR
- TEST_RESULT

Platform-specific diagnostics remain stored with their own platform unless a finding affects shared behavior.

---

## 17. No cross-platform overwrite rule

A platform worker must never perform a broad repository cleanup, refactor, mass rename, branch reset, or folder move that changes the other platform's area without explicit user authorization.

Before any repository-wide structural change:

1. checkpoint both platform states when they exist;
2. identify every path that will move/change;
3. preserve both verified baselines;
4. record the migration plan;
5. obtain explicit user direction if the change affects both platform implementations.

---

## 18. Build/output isolation

Keep platform artifacts separate.

Examples:

```text
artifacts/android/
artifacts/windows/
```

or equivalent release structures.

Never replace:

- an APK with a Windows artifact
- a Windows EXE/installer with an Android artifact
- one platform's source archive with the other's

Artifact names should identify platform and version.

Example:

```text
MyApp-Android-v0.8.0.apk
MyApp-Windows-v0.5.2.zip
```

---

## 19. Commit/change clarity

When practical, commit messages should identify the affected scope.

Examples:

- `android: add multi-file merge`
- `windows: add multi-file merge`
- `shared: add F-014 merge requirement`
- `shared: revise project file schema`

Avoid commits that mix unrelated Android, Windows, and shared changes.

---

## 20. Concurrent-work safety

If both agents may be working near the same time:

- platform-specific changes should remain in their own paths;
- shared-file edits must start from the latest version;
- do not force push or rewrite shared history;
- do not delete unknown files created by the other agent;
- do not assume an unfamiliar file is obsolete;
- resolve Git conflicts deliberately;
- checkpoint before large merges.

If separate branches are used, recommended naming is:

- `android-development`
- `windows-development`

with shared changes integrated deliberately.

Branches are optional; path ownership is mandatory.

---

## 21. Cross-platform disagreement

If the two implementations discover conflicting needs:

Do not force one platform to mimic the other if doing so harms that platform.

Record:

- shared desired behavior
- Android behavior/limitation
- Windows behavior/limitation
- reason for divergence

The goal is consistent user-facing capability where practical, not artificial implementation identity.

---

## 22. What should normally be shared

Normally share:

- product vision
- shared feature catalog
- feature definitions
- shared requirements
- shared terminology
- file/data interchange specifications
- cross-platform architecture decisions
- shared privacy/security requirements
- common algorithm descriptions when platform-neutral
- shared test intent
- cross-platform compatibility findings

Normally keep separate:

- source code
- build scripts
- dependencies
- platform UI implementation
- platform diagnostics
- platform test logs
- platform checkpoints
- verified baseline
- candidate versions
- release artifacts
- compiler/build errors
- platform-specific bugs

---

## 23. Required final principle

Each worker should be able to answer:

**What belongs to the whole product?**

versus:

**What belongs only to my platform implementation?**

Shared product information must be visible to both.

Platform implementation details must remain isolated enough that Android work cannot accidentally damage Windows work, and Windows work cannot accidentally damage Android work.

The repository should function as:

**shared product brain + separate Android workspace + separate Windows workspace.**
