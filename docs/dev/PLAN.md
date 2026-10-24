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

## Phase 2 — Piece supply and hold

Outcome: Play with predictable seven-kind bags and one-use hold.

Scope and boundary: R08 source/hold behavior; preserve classic movement, score and lifecycle. Landing/rotation controls are outside this phase.

Prerequisite: Phase 1 completed, reviewed, integrated and published. Target version: v0.2.0.

### Milestone 2.1 — Bag and hold play

Demonstrate C hold with a visible stored piece and bag-driven play.

Exit: R08 source consumption/hold allowance boundaries pass and the integrated hold path informs continue/amend/simplify/stop feedback A final milestone code review, focused regression checks, blocker repairs and a committed milestone report are required.

### Milestone 2.2 — Phase review

After every delivery milestone is complete and closed, review cross-milestone code, run phase acceptance and regressions, resolve blockers and commit the phase report. Verify seven-bag composition, empty/swap/rejected hold, blocked incoming spawn and restart in rules and both-browser UI Complete the explicit default-branch integration and merged-state checks before phase completion/release. If this completes the full intended task list, aggregate the final implementation report and outstanding TODOs.

## Phase 3 — Rotation and landing controls

Outcome: Use wall kicks, visible landing guidance and hard drop with a full grounded interval.

Scope and boundary: R09 kicks, ordinary projection, ghost, hard drop and interval timing. Preserve R08 supply/hold and classic clearing/scoring. Dynamic visibility is outside this phase.

Prerequisite: Phase 2 completed, reviewed, integrated and published. Target version: v0.3.0.

### Milestone 3.1 — Rotation and landing guidance

Demonstrate kicked rotations and a ghost predicting the ordinary landing.

Exit: R09 candidate order and ghost target/rendering pass; demonstrate wall/floor play for continue/amend/simplify/stop feedback A final milestone code review, focused regression checks, blocker repairs and a committed milestone report are required.

### Milestone 3.2 — Drop and grounded timing

Demonstrate hard-drop repositioning with a predictable full interval before lock.

Exit: R09 timer/drop/reset/suspension boundaries and both-browser interactions pass A final milestone code review, focused regression checks, blocker repairs and a committed milestone report are required.

### Milestone 3.3 — Phase review

After every delivery milestone is complete and closed, review cross-milestone code, run phase acceptance and regressions, resolve blockers and commit the phase report. Verify shared ghost/drop positions, kick-order/rejection fixtures, complete interval/reset semantics and both-browser regressions Complete the explicit default-branch integration and merged-state checks before phase completion/release. If this completes the full intended task list, aggregate the final implementation report and outstanding TODOs.

## Phase 4 — Conditional landing guidance

Outcome: Keep the ghost visible only when its landing lies wholly in the lower half.

Scope and boundary: R10 dynamic visibility over the existing projector; collision, drop and lock policies remain unchanged. No setting UI is required.

Prerequisite: Phase 3 completed, reviewed, integrated and published. Target version: v0.4.0.

### Milestone 4.1 — Dynamic ghost display

Demonstrate the ghost disappearing/reappearing as its projected occupied cells cross the midline.

Exit: R10 minimum-row 9/10 and straddling cases pass; demonstrate visual clarity for continue/amend/simplify/stop feedback A final milestone code review, focused regression checks, blocker repairs and a committed milestone report are required.

### Milestone 4.2 — Phase review

After every delivery milestone is complete and closed, review cross-milestone code, run phase acceptance and regressions, resolve blockers and commit the phase report. Verify midline/straddling hide-restore and unchanged hard-drop/timing behavior in rules and both-browser rendering Complete the explicit default-branch integration and merged-state checks before phase completion/release. If this completes the full intended task list, aggregate the final implementation report and outstanding TODOs.

## Phase 5 — Bonus-funded penetrator play

Outcome: Spend a four-line-clear bonus to guide a white piece through occupied cells into a legal landing.

Scope and boundary: R11 traversal/activation, bank and display with R09-R10 landing/timing/visibility interactions. No game settings or persistence.

Prerequisite: Phase 4 completed, reviewed, integrated and published. Target version: v0.5.0.

### Milestone 5.1 — Safe penetration mechanics

Demonstrate an activated piece crossing a barrier and locking into a legal cavity.

Exit: R11 terminal/movement/rotation/activation/hold invariants pass; demonstrate cavity reachability for continue/amend/simplify/stop feedback A final milestone code review, focused regression checks, blocker repairs and a committed milestone report are required.

### Milestone 5.2 — Bonus and visible penetrator experience

Demonstrate earned/spent bonuses and white penetrator play with matching ghost/drop.

Exit: R11 cap/award/appearance/indicator and R09-R10 integration cases pass in both browsers A final milestone code review, focused regression checks, blocker repairs and a committed milestone report are required.

### Milestone 5.3 — Phase review

After every delivery milestone is complete and closed, review cross-milestone code, run phase acceptance and regressions, resolve blockers and commit the phase report. Verify legal terminal insertion without destroyed cells, atomic 0–3 bonus lifecycle, disabled active hold, shared ghost/drop geometry and both-browser appearance/timing evidence Complete the explicit default-branch integration and merged-state checks before phase completion/release. If this completes the full intended task list, aggregate the final implementation report and outstanding TODOs.
