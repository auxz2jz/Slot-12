# Guided Testing Standard

This standard defines the reusable step-by-step testing system that accompanies the mandatory diagnostics architecture.

Tests must be created from the actual features of the program being inspected. Do not copy tests from another application merely because they are convenient examples.

## 1. Build tests from the program's actual features

Before defining the guided test catalog:

1. inspect the application's current controls and workflows;
2. identify important features, background jobs, file operations, outputs, and state transitions;
3. identify the internal signal that proves each feature actually worked;
4. identify visual/subjective behavior requiring human confirmation;
5. identify realistic failure and timeout conditions.

Create tests for THIS program's real functionality.

Examples from other programs are explanatory only and must not create nonexistent controls or workflows.

## 2. "Test This Version"

Each testable candidate should be able to provide a user-facing **Test This Version** workflow.

The guide should identify the exact version/build under test and present one test step at a time.

## 3. Persistent test session

Each guided test run should receive a unique:

- testSessionId
- test definition/version
- start/end timestamps
- app version/build
- device/platform
- current step
- attempt number
- overall result

Events generated during testing should carry the test session ID.

## 4. Test progress survives restart

Persist:

- current step
- completed steps
- results
- tester notes
- automatic evidence

Closing/reopening the app should not force the user to restart a long test sequence.

## 5. TestStep structure

Every test step should contain:

- stepId
- title
- exact user instruction
- expected result
- automatic verification rule when possible
- PASS criteria
- FAIL criteria
- timeout when appropriate
- evidence that should be collected
- whether a long-running operation is required
- what diagnostics to export on failure

Use the exact labels/buttons visible in the application.

## 6. Clear instructions

Preferred format:

**WHAT TO DO**  
Tap **Select Target**, draw a box around the moving object, then tap **Confirm Target**.

**EXPECTED RESULT**  
The box should remain on the selected object while it moves.

Avoid vague technical wording when a direct user instruction is possible.

## 7. Explicit no-operation tests

If a step does not require an encode/build/render/network operation, say so clearly:

**NO ENCODE REQUIRED FOR THIS TEST**

This prevents unnecessary work and confusion.

## 8. Apply controlled test settings

When a test requires many controlled values, provide **Apply Test Settings** where practical.

Automatically configure test parameters while preserving user-specific input selections that should remain.

## 9. Automatic PASS/FAIL verification

Where behavior is objectively measurable, the application should verify the expected result itself.

Example — playback:

1. user presses Play
2. Play request logged
3. player reports PLAYING
4. position advances by required amount
5. no relevant error occurs
6. PASS

Do not mark PASS because the button was pressed.

Example — export:

1. Export requested
2. destination opened
3. bytes written
4. stream closed successfully
5. output validated
6. PASS

## 10. Human visual confirmation

Some results are subjective or visual.

Examples:

- tracking stayed on the correct object
- texture alignment looks correct
- model looks correct
- UI layout looks correct

For these, combine objective automatic checks with human confirmation.

Example:

Automatic requirements:
- tracking ran long enough
- sufficient samples
- motion plausible
- no repeated rejected jumps

Then enable:
**Tracking Looks Correct**

## 11. Manual failure control

Every active test should provide an option such as:

**Expected Behavior Failed**

or:

**Problem**

This lets the tester report a visible failure the software cannot automatically detect.

Record:

- step
- tester explanation
- current screen/state
- active operation
- correlation ID
- recent diagnostics

## 12. Result source

Every test result should identify how it was determined.

Recommended values:

- AUTO_VERIFIED
- AUTO_FAIL
- MANUAL_PASS
- MANUAL_FAIL
- MANUAL_OVERRIDE
- BLOCKED
- SKIPPED
- UNTESTED

This separates software-proven success from tester-reported success.

## 13. False-positive protection

Tests must measure the intended behavior, not incidental activity.

Bad:
Tracking box moved → PASS.

Better:
Tracking movement remained plausible, stable samples accumulated, repeated jumps did not occur, and tester confirms the box stayed on the intended object.

Bad:
Process returned exit code 0 → PASS.

Better:
Process returned success, output exists, output is non-empty, and expected result properties validate.

## 14. Timeouts

Where appropriate, expected states should have a reasonable timeout.

Example:

Play requested.

Expected:
PLAYING + position advancement within 8 seconds.

If not:
TEST_RESULT=FAIL  
reason=Playback did not begin within timeout

## 15. Step progression

When a step begins, record:

TEST → STEP_STARTED

When it passes:

TEST → STEP_PASSED  
durationMs=...

Then advance to the next step.

On failure:

TEST → STEP_FAILED  
reason=...

Stop or branch according to the test definition.

At completion:

TEST → GUIDED_TEST_FINISHED  
overall=PASS/FAIL/PARTIAL

## 16. Tester notes

Allow an optional note on successful steps and a required/encouraged explanation for manual failures.

Useful prompt:

- what you tapped
- what you expected
- what actually happened

## 17. Test report

Generate both:

- human-readable TXT
- structured JSON when practical

Include:

- app version/build
- test session ID
- test start/end
- device/platform
- all test steps
- expected results
- actual results
- result source
- automatic evidence
- tester notes
- PASS/FAIL status
- first failed step
- errors
- relevant diagnostic event IDs
- current state snapshot

## 18. Report fingerprint

A report may include a fingerprint/hash to identify whether two exports represent the same captured state.

A fingerprint does not replace session/event/correlation IDs.

## 19. Test and diagnostics integration

A guided test should run inside a diagnostic session or otherwise be strongly correlated with diagnostics.

When a test fails, exported diagnostics should automatically include the relevant test session and recent event history.

## 20. Every important feature should have test coverage

For each important user-facing feature, background operation, or automatic process that can materially fail, define appropriate test coverage:

- what the tester does
- expected behavior
- internal signal proving the action occurred
- PASS criteria
- FAIL criteria
- what diagnostics to export if it fails

Do not mark a feature DONE solely because code was implemented.

## 21. Test evidence remains authoritative

If a summary reports PASS but raw chronological diagnostic evidence shows incorrect behavior, the raw evidence must trigger investigation.

Fix a false-positive test rather than changing reality to match the test.

## Final requirement

The guided testing system should make it possible for the user to follow simple on-screen steps while the application collects enough evidence to answer:

- what was tested
- what the user did
- what the software actually did
- what passed
- what failed
- why
- what diagnostics belong to that failure
