# Delivery plan

## Strategy and gates

Deliver bounded playable increments, integrating and checking regressions at every phase boundary. Each phase defines the behavior expected at its own release; a later capability may select a different default while preserving unaffected rules and explicitly supported strategies. Design/SPEC own the full intended behavior; phases own scoped exits.

Milestone demonstrations expose usability and integration risk for human continue/amend/simplify/stop feedback; routine progression within an authorized range needs no added approval. Tests, module/user documentation and required error paths travel with each delivery task. Every delivery milestone ends in code review/testing/report; a final dedicated milestone reviews the whole phase after delivery milestones close.

Phase releases use semantic versions, initially v0.1.0 with minor increments for capabilities and patches for corrective-only changes. After authorized implementation, release each completed verified integrated phase with matching version tag and root RELEASE-NOTES.md. No release is implied by document completion. Phase activation and publication obey repository policy.

Use injected source/time checks for rules and real browser input/rendering checks for adapters. Browser provisioning must follow BROWSER_DEV_POLICY.md before dependent acceptance; unavailable required evidence blocks the affected exit rather than reducing coverage.

## Phase 1 — Playable classic game

Outcome: Play a complete keyboard-controlled game from spawn through scoring, game over and restart.

Scope and boundary: R01–R07 classic behavior: uniform draws, basic rotation, fixed gravity and failed-down locking. No enhancement or configuration is required for this useful baseline.

Prerequisite: accepted integrated documentation and verified isolated browser provisioning before dependent browser checks. Target version: v0.1.0.

### Milestone 1.1 — Falling-piece play

Demonstrate visible pieces controlled by the keyboard and gravity on a served board. Tooling enables this path but is not itself the delivered capability.

Exit: R01–R04 geometry, spawn, blocked movement/rotation and active-time gravity are demonstrated; invite continue/amend/simplify/stop feedback on readability and input A final milestone code review, focused regression checks, blocker repairs and a committed milestone report are required.

### Milestone 1.2 — Complete game loop

Demonstrate locking, line scoring, game over and restart as a complete usable game.

Exit: R04–R07 lock/compaction/scoring/restart/focus and production delivery pass; demonstrate a scored line clear and restart to inform continue/amend/simplify/stop feedback A final milestone code review, focused regression checks, blocker repairs and a committed milestone report are required.

### Milestone 1.3 — Phase review

After every delivery milestone is complete and closed, review cross-milestone code, run phase acceptance and regressions, resolve blockers and commit the phase report. Verify a complete classic game, deterministic rule boundaries, both-browser interaction/rendering/restart evidence and a served production build Complete the explicit default-branch integration and merged-state checks before phase completion/release. If this completes the full intended task list, aggregate the final implementation report and outstanding TODOs.
