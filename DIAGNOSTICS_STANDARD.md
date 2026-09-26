# Built-In Diagnostics Standard

This standard defines diagnostic capabilities that should be incorporated into applications whenever technically appropriate.

## 1. Core principle

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

## 2. Central structured event logger

Use one central diagnostic event system whenever possible.

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

## 3. IDs and correlation

Generate separate IDs when applicable for:

- application session
- event
- user operation/correlation
- long-running operation/run
- guided test session
- test step/report

All events caused by an important user request should share a correlation ID.

## 4. Timing and order

Each event should include:

- sequence number
- UTC timestamp
- monotonic elapsed time

This supports reliable chronological reconstruction and performance timing.

## 5. User-action monitoring

Record important semantic actions before executing them, such as:

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

## 6. Optional raw touch/pointer trace

Raw touch/click coordinates may be captured when useful for UI diagnosis, but they are supplemental.

A touch proves only that input occurred at a location, not that a command succeeded.

Throttle/de-duplicate continuous pointer input so logs do not explode.

## 7. Keyboard privacy

Do not implement unrestricted raw keystroke logging by default.

Do not log passwords, codes, API keys, tokens, private messages, or sensitive typed text.

Prefer meaningful state changes, such as:

SETTING_CHANGE | bitrate 4500 → 800

## 8. Before/after state and settings

Where useful, record previous and resulting values.

Examples:

- bitrate: 4500 → 800
- mode: Automatic → Manual
- selectedClip: Clip 2 → Clip 3
- state: BUFFERING → PLAYING

## 9. Navigation and lifecycle

Where diagnostically useful, record:

- navigation requested
- destination screen reached
- Activity/app create/start/resume/pause/stop
- foreground/background
- orientation/configuration changes
- process/session start

These are important for rotation, backgrounding, file-picker returns, and long-running tasks.

## 10. Persistent rolling Action Trace

Maintain a bounded persistent trace on disk independent of any one operation.

Recommended design:

- current trace
- previous rotated trace
- fixed storage limit

It should normally survive ordinary app restarts/process restarts when app data remains.

Do not allow unlimited log growth.

## 11. Recent event buffer

Also maintain a bounded recent-event buffer, for example:

- last 500–2,000 events
- or last 5–15 minutes

Use it to quickly preserve immediate pre-failure history.

The persistent event stream remains authoritative.

## 12. Prompt persistence

Important diagnostic events should be flushed promptly enough that a crash does not erase the whole session.

Especially persist:

- operation starts
- major state changes
- errors
- test results
- stage completion
- crash markers

## 13. Long-running operation/run monitoring

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

## 14. Progress and stall detection

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

## 15. Dependency and precondition checks

Before fragile operations, record readiness such as:

- dependency/executable/library exists
- input exists
- working directory exists
- encoder/capability available
- prior stage exists
- storage/permission available
- dimensions/format supported

Precondition failures should be persisted as diagnostic events, not only displayed temporarily.

## 16. Result validation

Do not equate process completion with feature success.

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

## 17. Structured result reports

For complex operations, save structured operation/stage reports separately from the chronological event stream.

Event stream answers: **what happened and when?**

Result report answers: **what did the operation produce?**

Include useful metrics, warnings, readiness, quality values, and output details.

## 18. Rejection reasons

When processing multiple candidates/items, preserve individual rejection reasons.

Example:

Pair A/B — accepted  
Pair B/C — rejected: insufficient points  
Pair C/D — rejected: invalid geometry

This helps locate the first actual limitation.

## 19. Dependency invalidation

For staged pipelines, rebuilding an upstream stage must invalidate stale downstream results.

Record the invalidation.

Never leave old downstream results appearing current after a prerequisite changed.

## 20. Central error logging

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

## 21. Global crash preservation

When supported, install a global uncaught-exception handler.

Before normal platform crash handling continues, preserve:

- crash timestamp
- exception type/message
- full stack trace
- thread
- app version/build
- current screen
- active operation
- current guided test
- recent event history

Do not suppress normal OS crash behavior.

## 22. Optional resource/environment telemetry

For resource-intensive operations, optionally capture:

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

## 23. Diagnostic export

Provide an obvious **Export Diagnostics** control.

Prefer a single ZIP package for complex applications.

Recommended contents:

- README.txt
- summary.txt
- events.jsonl
- action_trace.txt
- test_report.txt
- test_report.json
- errors.txt
- crash.json / previous crash data
- device/app metadata
- operation report
- extended operation report
- relevant structured stage reports
- recent pre-failure events
- optional diagnostic images when useful

Do not automatically include the user's private source media/documents.

## 24. Normal and extended reports

Complex programs may provide:

**Normal Diagnostic Report** — routine debugging.

**Extended Diagnostic Report** — full retained logs, raw technical metadata, telemetry samples, stack traces, capability data, and detailed per-item analysis.

## 25. Export diagnostics must diagnose themselves

Record:

USER_ACTION → EXPORT_DIAGNOSTICS_PRESSED  
EXPORT → STARTED

Then either:

EXPORT → COMPLETED, bytes=...

or:

ERROR → DIAGNOSTIC_EXPORT_FAILED

Do not stop at "export started."

## 26. First-real-failure rule

During diagnosis, identify the earliest event where actual behavior diverged from expected behavior.

Do not assume the final visible error is the root cause.

Later symptoms should reference originating failure events when known.

## 27. Privacy/redaction

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

## 28. New-feature diagnostic requirement

Every important new user-facing feature should define:

1. user-action event
2. operation/request event
3. actual-result/state event
4. error handling
5. PASS criteria
6. FAIL criteria
7. guided test coverage
8. diagnostic export coverage

A feature is not diagnostically complete merely because code exists.
