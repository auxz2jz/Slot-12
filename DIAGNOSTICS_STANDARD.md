# Built-In Diagnostics Standard

This standard is **mandatory infrastructure for every software program developed under the Master Instruction Library**.

The exact implementation must be adapted to the application that actually exists. Do not copy controls, workflows, event names, or tests from another program unless they genuinely apply.

Diagnostics are not a temporary debugging add-on. They are part of the program's permanent architecture and must grow with the program.

## 1. Inspect THIS program before designing diagnostics

Before adding or extending diagnostics, inspect the actual program and identify its real behavior.

At minimum identify, when present:

- important buttons
- menus
- toolbar commands
- context-menu commands
- toggles and checkboxes
- sliders
- text submissions
- keyboard shortcuts
- mouse/touch/gesture actions
- file operations
- import/export operations
- navigation
- settings changes
- start/stop/cancel/retry actions
- processing commands
- background jobs
- automatic operations
- asynchronous callbacks
- major state transitions
- external-device commands
- outputs/artifacts that prove success
- current error handling
- current logs/tests
- important internal subsystems

Do not assume the application has Play, Pause, Seek, Zoom, Tracking, or any other example feature.

After inspection, create or update a **Diagnostic Coverage Map** that identifies for each important feature:

- user action or trigger
- software request/operation
- important internal state/progress
- success/result signal
- failure/error signal
- PASS criteria
- FAIL criteria
- diagnostic events to record
- guided test coverage

Preserve working architecture. Do not redesign stable software merely to make it resemble another project's diagnostics.

## 2. Core principle

Never treat a user action as proof of software success.

Diagnostics should separately identify:

1. **USER_ACTION** — what the user requested
2. **OPERATION_START / REQUEST** — what the program attempted
3. **STATE_TRANSITION / PROGRESS** — what actually happened internally
4. **OPERATION_RESULT** — what actually completed
5. **ERROR / FAILURE** — what went wrong
6. **TEST_VERIFICATION / TEST_RESULT** — why a test passed or failed

Example:

USER_ACTION → PLAY pressed  
OPERATION_START → playback requested  
STATE_TRANSITION → BUFFERING  
STATE_TRANSITION → PLAYING  
VERIFICATION → position advanced  
OPERATION_RESULT → success  
TEST_RESULT → PASS

A button press alone is never sufficient evidence of success.

## 3. Diagnostic session system

Create a diagnostic session system appropriate to the platform.

Each session should normally have:

- UUID or equivalent session ID
- optional human-readable session label
- UTC start timestamp
- monotonic elapsed-time origin
- sequence counter
- app version
- build/version code where applicable
- optional active test ID
- optional active test-step ID

Starting a guided test should normally start a fresh diagnostic session or otherwise create an unambiguous test-session boundary.

## 4. Central structured event logger

Use one central diagnostic event system whenever possible.

Prefer JSON Lines / JSONL or another structured append-friendly format that can be read both by people and tools.

Useful fields include:

- eventId
- sequenceNumber
- timestampUtc
- monotonicTimeMs
- appSessionId
- correlationId
- operationId/runId
- testSessionId
- testId/testStepId
- category
- severity
- screen/module/control
- userAction
- requestedOperation
- parameters
- stateBefore/stateAfter
- progress
- result/success
- durationMs
- errorCode
- errorType
- errorMessage
- stackTrace or reference
- relevantMetrics
- appVersion/buildNumber

Not every event needs every field.

## 5. IDs and correlation

Generate separate IDs when applicable for:

- application session
- event
- user operation/correlation
- long-running operation/run
- guided test session
- test step/report

All events caused by an important user request should share a correlation ID.

## 6. Timing and order

Each event should include:

- sequence number
- UTC timestamp
- monotonic elapsed time

This supports reliable chronological reconstruction and performance timing.

## 7. User-action monitoring

Instrument the important interactions that THIS program actually contains.

Record important semantic actions before executing them whenever practical, such as:

- buttons
- menus
- navigation
- Play/Pause/Stop
- Start/Cancel/Resume
- Save/Load
- Import/Export
- file selection/cancel
- confirmation
- slider commits
- toggles
- dropdown changes
- setting changes
- mode or project selection

Example:

USER_ACTION  
screen=Editor  
control=Split  
action=SPLIT_REQUESTED

Do not imply that Split succeeded.

## 8. Automatic/programmatic actions

Diagnostics must also cover important behavior that occurs without a direct button press.

Examples include:

- automatic processing
- background workers
- scheduled jobs
- auto-save
- reconnect/retry
- automatic validation
- automatic file discovery
- asynchronous responses
- external-device callbacks
- state changes triggered by the system rather than the user

The diagnostic system must cover important software behavior, not only UI clicks.

## 9. Optional raw touch/pointer trace

Raw touch/click coordinates may be captured when useful for UI diagnosis, but they are supplemental.

A touch proves only that input occurred at a location, not that a command succeeded.

Throttle/de-duplicate continuous pointer input so logs do not explode.

## 10. Keyboard privacy

Do not implement unrestricted raw keystroke logging by default.

Do not log passwords, codes, API keys, tokens, private messages, or sensitive typed text.

Prefer meaningful state changes, such as:

SETTING_CHANGE | bitrate 4500 → 800

## 11. Before/after state and settings

Where useful, record previous and resulting values.

Examples:

- bitrate: 4500 → 800
- mode: Automatic → Manual
- selectedClip: Clip 2 → Clip 3
- state: BUFFERING → PLAYING

## 12. Navigation and lifecycle

Where diagnostically useful, record:

- navigation requested
- destination screen reached
- Activity/app create/start/resume/pause/stop
- foreground/background
- orientation/configuration changes
- process/session start

These are important for rotation, backgrounding, file-picker returns, and long-running tasks.

## 13. Persistent rolling Action Trace

Maintain a bounded persistent trace on disk independent of any one operation.

Recommended design:

- current trace
- previous rotated trace
- fixed storage limit

It should normally survive ordinary app restarts/process restarts when app data remains.

Do not allow unlimited log growth.

## 14. Recent event buffer

Also maintain a bounded recent-event buffer, for example:

- last 500–2,000 events
- or last 5–15 minutes

Use it to quickly preserve immediate pre-failure history.

The persistent event stream remains authoritative.

## 15. Prompt persistence

Important diagnostic events should be flushed promptly enough that a crash does not erase the whole session.

Especially persist:

- operation starts
- major state changes
- errors
- test results
- stage completion
- crash markers

## 16. Long-running operation/run monitoring

Give complex operations a unique run/operation ID.

Record:

- start
- inputs/settings
- actual executed parameters/command
- phases
- progress
- warnings
- output growth/results
- cancellation
- failure
- completion
- duration

Applicable examples:

- encoding
- rendering
- 3D reconstruction
- scanning
- file conversion
- network jobs
- database jobs
- AI/ML processing

## 17. Progress and stall detection

Throttle or summarize high-frequency diagnostic sources such as continuous slider movement, dragging, pointer motion, sensors, progress callbacks, frame-by-frame processing, or rapidly repeated state samples.

Record enough information to diagnose behavior without generating excessive logs.

Record meaningful progress, such as:

- percentage
- processed count
- output bytes
- frames
- media time
- points/triangles
- successful/rejected candidates
- processing speed
- current stage

Detect cases where an operation is still running but making no meaningful progress.

Record watchdog/stall events with the last known progress.

Where practical, also use a lightweight UI/main-thread responsiveness watchdog.

## 18. Dependency and precondition checks

Before fragile operations, record readiness such as:

- dependency/executable/library exists
- input exists
- working directory exists
- encoder/capability available
- prior stage exists
- storage/permission available
- dimensions/format supported

Precondition failures should be persisted as diagnostic events, not only displayed temporarily.

## 19. Result validation

For every important action, identify the internal signal that proves the requested operation actually occurred.

Do not equate a button press, command selection, progress indicator, UI change, or process completion with feature success.

Examples:

Playback:
- PLAYING state reached
- position advances

Save/export:
- destination opened
- bytes written
- stream closed
- output exists when verifiable

Encode:
- successful process result
- output exists
- output non-empty
- output properties valid

3D processing:
- report exists
- expected artifact exists
- readiness flag true
- required quality metrics satisfied

## 20. Structured result reports

For complex operations, save structured operation/stage reports separately from the chronological event stream.

Event stream answers: **what happened and when?**

Result report answers: **what did the operation produce?**

Include useful metrics, warnings, readiness, quality values, and output details.

## 21. Rejection reasons

When processing multiple candidates/items, preserve individual rejection reasons.

Example:

Pair A/B — accepted  
Pair B/C — rejected: insufficient points  
Pair C/D — rejected: invalid geometry

This helps locate the first actual limitation.

## 22. Dependency invalidation

For staged pipelines, rebuilding an upstream stage must invalidate stale downstream results.

Record the invalidation.

Never leave old downstream results appearing current after a prerequisite changed.

## 23. Central error logging

Do not silently swallow important failures.

Important caught errors should preserve when appropriate:

- exception/error type
- code
- message
- full stack trace
- cause chain
- thread
- screen/module
- correlation/run ID
- current state
- relevant inputs
- elapsed time
- recent related events

Log lower-level subsystem failures before they become higher-level user-facing symptoms.

## 24. Global crash preservation

When supported, install a global uncaught-exception handler.

Before normal platform crash handling continues, attempt to preserve:

- crash timestamp
- exception type/message
- full stack trace
- thread
- app version/build
- current screen
- active operation
- current guided test
- current event-log filename/reference
- recent event history

Do not suppress normal OS crash behavior.

## 25. Device/environment and input metadata

Collect only environment and input metadata relevant to diagnosing THIS application.

Useful environment fields may include:

- foreground/background
- CPU
- memory
- child-process resources
- free RAM
- low-memory state
- free storage
- battery/charging
- battery/thermal temperature
- power-save state
- screen state

Use only where diagnostically valuable.

For files, projects, media, documents, datasets, or other inputs, record safe metadata needed for reproduction, such as:

- filename/display name
- type/format
- size
- dimensions
- item/record count
- relevant processing parameters

Do not automatically include sensitive original content in diagnostics.

## 26. Performance timing

Measure important operation durations with a monotonic clock.

Examples include:

- load
- save
- import
- export
- processing
- rendering
- conversion
- query
- analysis
- network call
- initialization

## 27. Diagnostic export

Provide an obvious **Export Diagnostics** control.

Prefer a single ZIP package for complex applications.

At the end of guided testing, also provide an **Export Test + Diagnostics** path when practical.

Recommended contents:

- README.txt
- summary.txt
- events.jsonl
- action_trace.txt
- test_report.txt
- guided_test_results.json or equivalent structured test results
- errors.txt
- crash.json / previous crash data
- device/app metadata
- operation report
- extended operation report
- relevant structured stage reports
- recent pre-failure events
- optional diagnostic images when useful

Do not automatically include the user's private source media/documents.

## 28. Diagnostic summary and machine-readable results

The export should include a human-readable summary containing:

- app version/build
- session ID
- test ID when applicable
- overall result
- step results
- step durations
- first failed step
- important errors
- safe input metadata
- relevant environment metadata
- filenames included in the package

Machine-readable guided-test results should include:

- test ID
- overall status
- completed/interrupted state
- current step if interrupted
- per-step results
- durations
- messages/evidence

## 29. Normal and extended reports

Complex programs may provide:

**Normal Diagnostic Report** — routine debugging.

**Extended Diagnostic Report** — full retained logs, raw technical metadata, telemetry samples, stack traces, capability data, and detailed per-item analysis.

## 30. Export diagnostics must diagnose themselves

Record:

USER_ACTION → EXPORT_DIAGNOSTICS_PRESSED  
EXPORT → STARTED

Then either:

EXPORT → COMPLETED, bytes=...

or:

ERROR → DIAGNOSTIC_EXPORT_FAILED

Do not stop at "export started."

## 31. First-real-failure rule

During diagnosis, identify the earliest event where actual behavior diverged from expected behavior.

Do not assume the final visible error is the root cause.

Later symptoms should reference originating failure events when known.

## 32. Privacy/redaction

Pass diagnostic information through a sanitizer/redactor before persistence/export.

Never persist unnecessary:

- passwords
- API keys
- auth tokens
- secret URLs
- precise location
- personal account data
- full private paths
- sensitive content URIs

Prefer generated IDs and safe metadata.

Diagnostics should remain local until the user explicitly exports/shares them.

## 33. Mandatory per-feature diagnostic requirement

Every important new user-facing feature, background feature, or automatic operation should add or update:

1. user-action or trigger event
2. operation/request event
3. state/progress events
4. actual-result event
5. error handling
6. request/operation correlation when asynchronous
7. relevant before/after state
8. timing where useful
9. PASS criteria
10. FAIL criteria
11. guided test coverage
12. diagnostic export coverage

A feature is not diagnostically complete merely because code exists.

## 34. Development/error-correction workflow

When a failure occurs:

1. Read the diagnostic summary.
2. Identify the failed test step or failed operation.
3. Inspect chronological events.
4. Find the first abnormal event/state.
5. Inspect error/exception details.
6. Make the smallest evidence-based fix.
7. Retest.
8. Save the result and update project memory/checkpoint.

Do not randomly rewrite unrelated working code.

## Final diagnostic requirement

The diagnostic system should allow a developer to reconstruct:

**WHAT THE USER DID OR WHAT TRIGGERED THE OPERATION**

→ **WHAT THE SOFTWARE ATTEMPTED**

→ **WHAT INTERNAL STATE/PROGRESS OCCURRED**

→ **WHAT ACTUALLY HAPPENED**

→ **WHERE THE FIRST REAL FAILURE OCCURRED**

→ **WHY THE TEST PASSED OR FAILED**

without relying on the user to remember ordinary diagnostic details.
