# Implementation tasks

Execute only an authorized range after preparation integration and eligible-phase activation. PLAN owns phase/milestone exits; SPEC owns behavior; layout owns intended edit locations. Task tests and documentation are part of each result.

## Phase 1 — Playable classic game

- [ ] Phase 1 — Playable classic game
    - [ ] Milestone 1.1 — Falling-piece play
        - [ ] T-001 — Establish the static build and isolated browser harness
            Scope: package/build scripts, TypeScript/Vite entry, test configuration and setup guide; provision the two managed browser targets under BROWSER_DEV_POLICY.md before dependent checks. Depends on: none after preparation integration and phase activation.
            Evidence: Type checking and an empty-shell production build succeed; runner/binary identities, isolated paths and repeated lifecycle/interaction/screenshot provisioning checks are retained. No gameplay acceptance claim. Maintain affected module/user documentation with the result.
        - [ ] T-002 — Implement board geometry and reproducible piece spawning
            Scope: core types, geometry, board validity and uniform/injected source with focused rule tests. Depends on: T-001.
            Evidence: R01 masks/rotations and R02 source consumption, spawn and blocked-spawn fixtures pass without browser APIs. Maintain affected module/user documentation with the result.
        - [ ] T-003 — Integrate keyboard movement gravity and board rendering
            Scope: reducer movement, controller timing, input and Canvas/HTML adapters for the falling-piece slice. Depends on: T-002.
            Evidence: R03–R04 legal/rejected actions and 999/1000 ms boundaries pass; both browsers visibly move/rotate/fall through actual keyboard events. Maintain affected module/user documentation with the result.
        - [ ] T-004 — Review, test and report milestone 1.1
            Depends on: T-001, T-002, T-003. Review the milestone implementation and collaborating modules, run its PLAN exit checks and regressions, repair blockers, and commit the report with findings and TODOs (None when empty).
            Report: `docs/dev/reports/phases/1/1.1.md`.
    - [ ] Milestone 1.2 — Complete game loop
        - [ ] T-005 — Implement atomic locking row clearing and scoring
            Scope: board compaction and reducer lock/score/next-spawn transitions with deterministic fixtures. Depends on: T-004.
            Evidence: R04–R05 simultaneous 1–4-row clears, exact scores, rejected insertions and single-consumption next spawn pass. Maintain affected module/user documentation with the result.
        - [ ] T-006 — Complete game status restart and focus lifecycle
            Scope: controller suspension/disposal and HTML game-over, score, controls and restart integration. Depends on: T-005.
            Evidence: R06 repeated restart, blocked spawn, no background catch-up, keyboard/button restart and listener/frame cleanup pass. Maintain affected module/user documentation with the result.
        - [ ] T-007 — Verify and document the served production game
            Scope: browser game-path fixtures, screenshot evidence, production build and README play/build instructions. Depends on: T-006.
            Evidence: R01–R07 applicable end-to-end cases pass in Chromium and managed Firefox; inspect Canvas output, serve the production build, and document actual commands/limits. Maintain affected module/user documentation with the result.
        - [ ] T-008 — Review, test and report milestone 1.2
            Depends on: T-005, T-006, T-007. Review the milestone implementation and collaborating modules, run its PLAN exit checks and regressions, repair blockers, and commit the report with findings and TODOs (None when empty).
            Report: `docs/dev/reports/phases/1/1.2.md`.
    - [ ] Milestone 1.3 — Phase review
        - [ ] T-009 — Review, test and report phase 1
            Depends on: completion/closure of delivery milestones 1.1, 1.2, including their review tasks. Review cross-milestone code, run all phase exits/regressions, repair blockers and aggregate findings/TODOs. Commit the phase report and refresh HANDOFF; verify integration and the planned phase release. If this is the last phase in the complete owning list, also write the final implementation report and aggregate all unresolved TODOs with provenance/options.
            Report: `docs/dev/reports/phases/1/PHASE-REPORT.md`; final report when applicable: `docs/dev/reports/IMPLEMENTATION-REPORT.md`.

## Phase 2 — Piece supply and hold

- [ ] Phase 2 — Piece supply and hold
    - [ ] Milestone 2.1 — Bag and hold play
        - [ ] T-010 — Implement bag-driven piece supply
            Scope: source strategy and deterministic unit fixtures. Depends on: T-009 and Phase 1 integration/closure.
            Evidence: Each bag contains seven unique kinds; deterministic shuffle/draw and restart boundaries pass. Maintain affected module/user documentation with the result.
        - [ ] T-011 — Integrate one-use hold and its display
            Scope: hold reducer, input/snapshot/view and browser cases. Depends on: T-010.
            Evidence: R08 empty/swap/denied hold draw counts, blocked spawn, display and reset pass in rule and browser checks. Maintain affected module/user documentation with the result.
        - [ ] T-012 — Review, test and report milestone 2.1
            Depends on: T-010, T-011. Review the milestone implementation and collaborating modules, run its PLAN exit checks and regressions, repair blockers, and commit the report with findings and TODOs (None when empty).
            Report: `docs/dev/reports/phases/2/2.1.md`.
    - [ ] Milestone 2.2 — Phase review
        - [ ] T-013 — Review, test and report phase 2
            Depends on: completion/closure of delivery milestones 2.1, including their review tasks. Review cross-milestone code, run all phase exits/regressions, repair blockers and aggregate findings/TODOs. Commit the phase report and refresh HANDOFF; verify integration and the planned phase release. If this is the last phase in the complete owning list, also write the final implementation report and aggregate all unresolved TODOs with provenance/options.
            Report: `docs/dev/reports/phases/2/PHASE-REPORT.md`; final report when applicable: `docs/dev/reports/IMPLEMENTATION-REPORT.md`.

## Phase 3 — Rotation and landing controls

- [ ] Phase 3 — Rotation and landing controls
    - [ ] Milestone 3.1 — Rotation and landing guidance
        - [ ] T-014 — Implement deterministic wall-kick rotation
            Scope: geometry rotation resolver and board fixtures. Depends on: T-013 and Phase 2 integration/closure.
            Evidence: R09 ordered candidates, I and O behavior and exact rejection pass. Maintain affected module/user documentation with the result.
        - [ ] T-015 — Integrate shared landing projection and ghost
            Scope: pure projector and snapshot/Canvas adapter. Depends on: T-014.
            Evidence: Ordinary obstruction fixtures, coincident suppression, no board/source mutation and visible ghost positions pass. Maintain affected module/user documentation with the result.
        - [ ] T-016 — Review, test and report milestone 3.1
            Depends on: T-014, T-015. Review the milestone implementation and collaborating modules, run its PLAN exit checks and regressions, repair blockers, and commit the report with findings and TODOs (None when empty).
            Report: `docs/dev/reports/phases/3/3.1.md`.
    - [ ] Milestone 3.2 — Drop and grounded timing
        - [ ] T-017 — Implement full grounded-interval timing
            Scope: controller lock policy, reducer grounding events and fake-clock cases. Depends on: T-016.
            Evidence: 999/1000ms, equal-time ordering, no grounded reset, leave-support/reground and suspension pass. Maintain affected module/user documentation with the result.
        - [ ] T-018 — Connect hard drop to shared projection and timing
            Scope: Space input and reducer/controller integration. Depends on: T-017.
            Evidence: Target matches ghost; mid-cycle grounding gets a full interval; repeated or zero-distance drops cannot postpone lock. Maintain affected module/user documentation with the result.
        - [ ] T-019 — Verify enhanced play and update controls documentation
            Scope: both-browser app scenarios, rendering evidence and README controls. Depends on: T-018.
            Evidence: R08-R09 enhancement interactions and affected classic regressions pass in the served production build. Maintain affected module/user documentation with the result.
        - [ ] T-020 — Review, test and report milestone 3.2
            Depends on: T-017, T-018, T-019. Review the milestone implementation and collaborating modules, run its PLAN exit checks and regressions, repair blockers, and commit the report with findings and TODOs (None when empty).
            Report: `docs/dev/reports/phases/3/3.2.md`.
    - [ ] Milestone 3.3 — Phase review
        - [ ] T-021 — Review, test and report phase 3
            Depends on: completion/closure of delivery milestones 3.1, 3.2, including their review tasks. Review cross-milestone code, run all phase exits/regressions, repair blockers and aggregate findings/TODOs. Commit the phase report and refresh HANDOFF; verify integration and the planned phase release. If this is the last phase in the complete owning list, also write the final implementation report and aggregate all unresolved TODOs with provenance/options.
            Report: `docs/dev/reports/phases/3/PHASE-REPORT.md`; final report when applicable: `docs/dev/reports/IMPLEMENTATION-REPORT.md`.

## Phase 4 — Conditional landing guidance

- [ ] Phase 4 — Conditional landing guidance
    - [ ] Milestone 4.1 — Dynamic ghost display
        - [ ] T-022 — Implement occupied-cell ghost visibility
            Scope: pure visibility predicate and landing fixtures. Depends on: T-021 and Phase 3 integration/closure.
            Evidence: Exact row 9/10, straddling and no-landing/current-pose suppression cases pass without changing the projector. Maintain affected module/user documentation with the result.
        - [ ] T-023 — Integrate and verify dynamic ghost presentation
            Scope: snapshot/view wiring, both-browser scenarios and controls guidance. Depends on: T-022.
            Evidence: Hide/restore appears in Canvas; hard-drop target and timing remain unchanged under R09-R10. Maintain affected module/user documentation with the result.
        - [ ] T-024 — Review, test and report milestone 4.1
            Depends on: T-022, T-023. Review the milestone implementation and collaborating modules, run its PLAN exit checks and regressions, repair blockers, and commit the report with findings and TODOs (None when empty).
            Report: `docs/dev/reports/phases/4/4.1.md`.
    - [ ] Milestone 4.2 — Phase review
        - [ ] T-025 — Review, test and report phase 4
            Depends on: completion/closure of delivery milestones 4.1, including their review tasks. Review cross-milestone code, run all phase exits/regressions, repair blockers and aggregate findings/TODOs. Commit the phase report and refresh HANDOFF; verify integration and the planned phase release. If this is the last phase in the complete owning list, also write the final implementation report and aggregate all unresolved TODOs with provenance/options.
            Report: `docs/dev/reports/phases/4/PHASE-REPORT.md`; final report when applicable: `docs/dev/reports/IMPLEMENTATION-REPORT.md`.

## Phase 5 — Bonus-funded penetrator play

- [ ] Phase 5 — Bonus-funded penetrator play
    - [ ] Milestone 5.1 — Safe penetration mechanics
        - [ ] T-026 — Implement penetrator transit and terminal projection
            Scope: landing/collision-policy functions, board boundary and cavity fixtures. Depends on: T-025 and Phase 4 integration/closure.
            Evidence: Deepest legal cavity, crossed-cell preservation, in-bounds transit and no-terminal rejection pass with ordinary regression fixtures. Maintain affected module/user documentation with the result.
        - [ ] T-027 — Integrate activation hold rejection and terminal grounding
            Scope: active-mode reducer, B/C input guards and controller grounding transitions. Depends on: T-026.
            Evidence: Single activation, no-spend rejections, inert hold, timer preservation/cancellation and final nonoverlapping insertion pass. Maintain affected module/user documentation with the result.
        - [ ] T-028 — Review, test and report milestone 5.1
            Depends on: T-026, T-027. Review the milestone implementation and collaborating modules, run its PLAN exit checks and regressions, repair blockers, and commit the report with findings and TODOs (None when empty).
            Report: `docs/dev/reports/phases/5/5.1.md`.
    - [ ] Milestone 5.2 — Bonus and visible penetrator experience
        - [ ] T-029 — Implement capped bonus earning and restart lifecycle
            Scope: lock/clear/award reducer transaction and bank fixtures. Depends on: T-028.
            Evidence: Exactly-four award once, 0–3 cap, spent/restored bank and restart reset pass without score duplication. Maintain affected module/user documentation with the result.
        - [ ] T-030 — Render white cells and the bonus indicator
            Scope: cell appearance metadata, view/snapshot and accessible count label. Depends on: T-029.
            Evidence: Active/settled/ghost penetrator cells are white, regular pieces nonwhite, and symbols/count update on earn/spend. Maintain affected module/user documentation with the result.
        - [ ] T-031 — Verify penetrator play across landing controls
            Scope: both-browser barrier/cavity scenarios, screenshot evidence and README controls. Depends on: T-030.
            Evidence: Ghost/drop parity, dynamic visibility, active hold rejection, lock delay and ordinary game regressions pass on served build. Maintain affected module/user documentation with the result.
        - [ ] T-032 — Review, test and report milestone 5.2
            Depends on: T-029, T-030, T-031. Review the milestone implementation and collaborating modules, run its PLAN exit checks and regressions, repair blockers, and commit the report with findings and TODOs (None when empty).
            Report: `docs/dev/reports/phases/5/5.2.md`.
    - [ ] Milestone 5.3 — Phase review
        - [ ] T-033 — Review, test and report phase 5
            Depends on: completion/closure of delivery milestones 5.1, 5.2, including their review tasks. Review cross-milestone code, run all phase exits/regressions, repair blockers and aggregate findings/TODOs. Commit the phase report and refresh HANDOFF; verify integration and the planned phase release. If this is the last phase in the complete owning list, also write the final implementation report and aggregate all unresolved TODOs with provenance/options.
            Report: `docs/dev/reports/phases/5/PHASE-REPORT.md`; final report when applicable: `docs/dev/reports/IMPLEMENTATION-REPORT.md`.

## Phase 6 — Session game configuration

- [ ] Phase 6 — Session game configuration
    - [ ] Milestone 6.1 — Validated game controls
        - [ ] T-034 — Implement typed page-session settings
            Scope: application settings schema/model and pure validation/lifecycle fixtures. Depends on: T-033 and Phase 5 integration/closure.
            Evidence: Six fields/defaults/allowed values, invalid rejection and restart-versus-reload model lifetime pass without storage access. Maintain affected module/user documentation with the result.
        - [ ] T-035 — Connect controls availability guards and lock-policy latching
            Scope: HTML form, controller effective policy and hold/drop/ghost guards. Depends on: T-034.
            Evidence: All four locking combinations, immediate disable guards, mid-piece edits and held-state retention pass; controls keep native focus behavior. Maintain affected module/user documentation with the result.
        - [ ] T-036 — Review, test and report milestone 6.1
            Depends on: T-034, T-035. Review the milestone implementation and collaborating modules, run its PLAN exit checks and regressions, repair blockers, and commit the report with findings and TODOs (None when empty).
            Report: `docs/dev/reports/phases/6/6.1.md`.
    - [ ] Milestone 6.2 — Preview and configuration interactions
        - [ ] T-037 — Integrate non-consuming queue preview
            Scope: source peek/snapshot, preview rendering and deterministic queue fixtures. Depends on: T-036.
            Evidence: Toggle and repeated render leave sequence unchanged; empty/swap hold consume the expected next kind; disabled preview hides immediately. Maintain affected module/user documentation with the result.
        - [ ] T-038 — Verify the configuration matrix and document controls
            Scope: both-browser native-control/focus/reload/penetrator scenarios and README settings guide. Depends on: T-037.
            Evidence: Defaults, valid/invalid changes, all lock modes, restart retention, reload reset and absence of persistence access pass on served build. Maintain affected module/user documentation with the result.
        - [ ] T-039 — Review, test and report milestone 6.2
            Depends on: T-037, T-038. Review the milestone implementation and collaborating modules, run its PLAN exit checks and regressions, repair blockers, and commit the report with findings and TODOs (None when empty).
            Report: `docs/dev/reports/phases/6/6.2.md`.
    - [ ] Milestone 6.3 — Phase review
        - [ ] T-040 — Review, test and report phase 6
            Depends on: completion/closure of delivery milestones 6.1, 6.2, including their review tasks. Review cross-milestone code, run all phase exits/regressions, repair blockers and aggregate findings/TODOs. Commit the phase report and refresh HANDOFF; verify integration and the planned phase release. If this is the last phase in the complete owning list, also write the final implementation report and aggregate all unresolved TODOs with provenance/options.
            Report: `docs/dev/reports/phases/6/PHASE-REPORT.md`; final report when applicable: `docs/dev/reports/IMPLEMENTATION-REPORT.md`.

## Phase 7 — Persistent shareable configuration

- [ ] Phase 7 — Persistent shareable configuration
    - [ ] Milestone 7.1 — Restored configuration
        - [ ] T-041 — Implement the six-field URL codec and resolver
            Scope: pure settings codecs/resolution and raw-source table fixtures. Depends on: T-040 and Phase 6 integration/closure.
            Evidence: Canonical bool/enum tokens, malformed/duplicate inputs, independent URL-cookie-default selection and explicit-default intents pass. Maintain affected module/user documentation with the result.
        - [ ] T-042 — Integrate cookie restoration before game startup
            Scope: browser cookie reader/writer and bootstrap coordinator. Depends on: T-041.
            Evidence: Valid explicit URL values save even at default; omitted/invalid URL retains valid cookies; first active piece latches restored policies; blocked cookies are nonfatal. Maintain affected module/user documentation with the result.
        - [ ] T-043 — Review, test and report milestone 7.1
            Depends on: T-041, T-042. Review the milestone implementation and collaborating modules, run its PLAN exit checks and regressions, repair blockers, and commit the report with findings and TODOs (None when empty).
            Report: `docs/dev/reports/phases/7/7.1.md`.
    - [ ] Milestone 7.2 — Saved and shareable changes
        - [ ] T-044 — Persist control changes to cookies and URL
            Scope: settings change coordinator and URL/history adapter. Depends on: T-043.
            Evidence: One field writes its canonical cookie/query value; unrelated settings/hash survive; no navigation/history entry; write failures are independent and current-piece latching is preserved. Maintain affected module/user documentation with the result.
        - [ ] T-045 — Verify persisted settings and document sharing behavior
            Scope: both-browser cookie/URL/reload/error scenarios, production build and README configuration guide. Depends on: T-044.
            Evidence: All six mixed-source/default/omission cases, retained-cookie new pages, failed persistence and complete affected gameplay regressions pass; document actual behavior and limits. Maintain affected module/user documentation with the result.
        - [ ] T-046 — Review, test and report milestone 7.2
            Depends on: T-044, T-045. Review the milestone implementation and collaborating modules, run its PLAN exit checks and regressions, repair blockers, and commit the report with findings and TODOs (None when empty).
            Report: `docs/dev/reports/phases/7/7.2.md`.
    - [ ] Milestone 7.3 — Phase review
        - [ ] T-047 — Review, test and report phase 7
            Depends on: completion/closure of delivery milestones 7.1, 7.2, including their review tasks. Review cross-milestone code, run all phase exits/regressions, repair blockers and aggregate findings/TODOs. Commit the phase report and refresh HANDOFF; verify integration and the planned phase release. If this is the last phase in the complete owning list, also write the final implementation report and aggregate all unresolved TODOs with provenance/options.
            Report: `docs/dev/reports/phases/7/PHASE-REPORT.md`; final report when applicable: `docs/dev/reports/IMPLEMENTATION-REPORT.md`.
