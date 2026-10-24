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
