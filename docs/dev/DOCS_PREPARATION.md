# Main documentation preparation record

## Current control

- Mode and source: autodoc/autoloop, explicitly requested by the user on 2026-10-24 for this repository and the six scopes below.
- Multipass delivery: strict; no flexible integration permission was given. Later scopes append bounded phases/tasks and preserve earlier IDs, names, scope, milestone membership, order, dependencies and exits.
- Current pass: 3, cumulative scope 3; DOCS-005 completion recorded below. After its verified publication/issue closure, continue to DOCS-006.
- Approval basis: the user approved the first PROJECT and presented cross-scope decisions on 2026-10-24. Later PROJECT approvals are waived, announced in chat before that first checkpoint; ARCHITECTURE through TASKS checkpoints are preapproved by the selected mode. This approval authorizes the predefined sequence/resets. The presented product choices are accepted within their stated applicability.
- Delegation after first approval: resolve later material decisions within the predefined scope, record the rationale and explain consequential choices in chat without adding approval pauses.
- Stop: after Pass 6 DOCS-008 publication and issue closure, before DOCS-009, whole-package QC, documentation integration or implementation. Implementation will require subsequent authorization; releases apply only after verified integrated implementation phases.
- Tracking: enabled for documentation preparation and implementation by existing root AGENTS.md. Publication: push/read back each owned commit before continuing. Reporting: Standard after Explicit bootstrap; Minimal and Explicit remain selectable.
- Package: repository-bundled SDD Manager 0.15.2 at original commit `86aa03f573df4268fa32449523a253e9b9a217ff`; assets and procedures come from that pinned source.

## Scope sequence

1. **Pass 1 — Playable classic baseline.** Falling tetrominoes, movement, rotation, gravity, line clearing, scoring, game over and restart. Keep the baseline minimal and coherent; do not adopt future enhancements or configuration into its product documents.
2. **Pass 2 — Scope extension: Gameplay enhancements.** Add hold, ghost, 7-bag randomization, wall kicks, hard drop and default locking after a full gravity interval from grounding, including grounding through hard drop.
3. **Pass 3 — Scope extension: Conditional ghost visibility.** Hide the ghost when its projected landing is above the board midline; restore it in the lower half. Resolve straddling according to the proposed decision below.
4. **Pass 4 — Scope extension: Penetrator mode.** With a bonus, a hotkey activates penetration for the current piece until lock. Traverse occupied cells without destroying them; lock entirely in bounds without overlap. Hard drop and ghost share the lowest valid landing. Four-line clears grant one bonus, capped at three. Spend one per activation. Render the penetrator white, regular pieces nonwhite, and show one side-panel symbol per bonus, updated on earning/spending.
5. **Pass 5 — Scope extension: Session-only game configuration.** Side-panel controls: ghost hide/dynamic/show; hard drop on/off; next preview on/off; hard-drop lock follows general/immediate; general lock failed-down/full-grounded-interval; hold on/off. Add next preview here. Sensible defaults; current page session only, no persistence across reloads.
6. **Pass 6 — Scope extension: Configuration persistence.** Resolve each setting independently from an explicit valid URL parameter, then a valid saved cookie, then default. Changes save the corresponding cookie and update the URL parameter. Valid explicit URL values also save to cookies when equal to defaults. Omitted parameters retain saved cookies. Preserve unrelated query parameters and fragments.

## Per-pass decisions

The user approved the initial PROJECT proposal and presented cross-scope decisions on 2026-10-24. Retain their agent origin and rationale; they are accepted for the applicable scopes. Future choices stay here until applicable.

| Pass / decision | Resolution and rationale | Basis / source | Applicability / status |
| --- | --- | --- | --- |
| 1 / Product and delivery | Local single-player keyboard browser game; TypeScript, Canvas board, HTML status/controls, static build and no backend. Keep logic independent of browser APIs. A DOM-only board is possible but Canvas makes falling-piece overlays direct; a framework adds little for this small UI. | Agent recommendation for first PROJECT review. | All scopes; accepted by initial review. |
| 1 / Minimal baseline | Standard seven tetrominoes on a 10 by 20 visible board, basic rotation without kicks, independent uniform draws, fixed gravity and failed-down locking; score line clears and offer restart. No level progression is required by this seed. | Agent proposal consistent with the minimal baseline; later enhancements remain separate. | Pass 1; accepted by initial review. |
| 1 / Browser targets | Chromium primary and Playwright-managed Firefox secondary; headless execution sufficient for UI interaction and screenshots. Apply root BROWSER_DEV_POLICY.md, with isolated provisioning evidence before dependent browser work. Document preparation installs no browsers. | Existing repository policy; Playwright is an agent-proposed test runner. | All scopes; policy active, runner accepted by initial review. |
| 2 / Timing and reset | Start a full current gravity interval when grounded. Successful movement/rotation while still grounded does not renew it. Leaving support cancels the deadline; regrounding starts a fresh interval. Suspend active time while the page is hidden or unfocused and resume without catch-up. Hard drop follows the same policy by default. | Agent proposal addressing the user's explicit clarification request. | Passes 2–6; accepted by initial review. |
| 3 / Midline | Show a dynamic ghost only if all projected cells are in the lower half: on a 20-row board, minimum occupied row is at least 10, with zero-based rows. A straddling projection is hidden. | Agent proposal; unambiguous geometric interpretation avoids partial upper-half ghosts. | Passes 3–6; accepted by initial review. |
| 4 / Activation and hold | Activation is one-way for the current piece. Reject repeat activation or absent bonus without state change. Hold is disabled while penetration is active. Spend a bonus only on successful activation; preserve the normal per-piece hold allowance. | Agent proposal to make bonus lifetime and hold behavior explicit. | Passes 4–6; accepted by initial review. |
| 4 / Safe penetration and locking | Only board bounds block transit; occupied cells remain unchanged. Select the deepest nonoverlapping in-bounds placement at or below the current pose for the current column/orientation. Ghost and hard drop use that same target. Reject activation/movement/rotation that leaves no valid terminal placement below; do not spend on failed activation. Lock delay starts only at that terminal placement and never merges an overlapping pose. Activation cancels a prior deadline only if it leaves the piece ungrounded; it does not refresh a deadline when the terminal position is unchanged. | Agent proposal to prevent trapped or illegal locks during occupied-cell traversal. | Passes 4–6; accepted by initial review. |
| 4 / Bonuses and rendering | Award one bonus for a four-line clear after lock, capped at three. Restart clears the bank; repeated activation cannot spend twice. Keep penetrator cells white after lock, with ordinary settled-cell collision behavior; regular pieces remain nonwhite. | User supplies award/cap/white appearance; agent proposes restart and settled-cell behavior. | Passes 4–6; supplementary details accepted by initial review. |
| 5 / Defaults | Dynamic ghost, hard drop enabled, next preview enabled, full-interval general lock, hard drop follows general, hold enabled. These preserve the preceding gameplay capabilities. | Agent recommendation; user requests sensible defaults. | Passes 5–6; accepted by initial review. |
| 5 / Live settings | Ghost, preview, hold availability and hard-drop availability update immediately. Retain a stored hold piece while hold is disabled. Lock policies are latched at spawn and changes apply to the next piece, avoiding retroactive locks or deadline resets. Disabling hold does not restore a consumed hold allowance; penetrator hold remains forbidden. | Agent proposal addressing configuration changes during gameplay. | Passes 5–6; accepted by initial review. |
| 6 / Invalid inputs and persistence | Invalid URL values fall through to valid cookies, then defaults; never save an invalid URL value over a valid cookie. Decode safely and reject ambiguous duplicate setting parameters. Validate each setting independently. Cookie failure leaves the game usable with in-memory settings and URL updates. Update URL with replaceState, preserving unrelated parameters/hash. | Agent recommendation; user specifies per-setting precedence and explicit-value saving. | Pass 6; accepted by initial review. |
| 1 / Version policy | First playable phase release v0.1.0; later verified capability increments increase minor versions during initial development, patches for corrective-only releases. Publish release notes and tags only after authorized implementation phases are verified and integrated into the actual default branch. No release for document preparation. | Agent version choice under the user's explicit versioning/release instruction. | Implementation; version policy accepted by initial review, release requirement confirmed. |

## Cross-scope decisions

| Concern | Decision / applicable scopes | Reconsideration condition |
| --- | --- | --- |
| Timing and collision | Pass 2 timing plus Pass 4 safe terminal-position rules govern penetration, ghost and hard drop together; Pass 5 policy latching preserves an active piece's contract. | Change to gravity, collision, timing, or live-policy semantics. |
| Ghost visibility and geometry | Pass 3 uses the occupied cells of the landing calculated under the active normal/penetrator collision rules; Pass 5 selects the visibility policy without changing the target. | Change to board dimensions, landing rules or midline definition. |
| Hold and bonuses | Pass 4 active penetrator prohibits hold even when Pass 5 enables hold; bonus spending and hold-per-piece limits remain independent. | Human changes penetrator lifetime or hold behavior. |
| Configuration lifetime | Pass 5 is page-local; only Pass 6 adds cookies/URL. Restart resets game state but retains selected settings within the page. | Additional configuration sources or reload behavior. |
| Strict delivery preservation | Capture phase/task ranges as each pass establishes them; later PLAN/TASKS revisions must compare protected entries with the retained earlier Git versions. Phase 1 / milestones 1.1–1.3 / T-001–T-009 are established; preserve their exact PLAN/TASKS blocks in later scopes. | Explicit flexible-multipass permission or a conflict requiring human direction. |

## Decisions and document evidence

| Task | Current evidence / decision basis |
| --- | --- |
| DOCS-001 | Completed in `ac69cd181d06f21ad6ed905bfc9317e868ffcb57`, pushed and exactly read back on design-docs/main with confirmed process cleanup. Four root files unchanged, exact bundled task definitions with only DOCS-001 checked, this evidence record and HANDOFF committed together. Issue 1 closed and freshly read back as completed in milestone 1. |
| DOCS-002 | Pass 1 PROJECT proposal at the content identity below, with README navigation. Reviewed against the minimal seed, current-only scope, document boundary, root browser policy and proposed constraints. PROJECT SHA-256 `2f71599a56c88431b4927d7ee6d6fbc8d1cf243324ae38e60feb6b02cfaf5f53`. User approved this exact published proposal and the cross-scope choices on 2026-10-24; completion and issue closure follow. Cross-scope proposals above are part of the first review, with future requirements retained here. |
| DOCS-003–008 | Authorized ordered authoring/QC following first PROJECT approval. Subsequent checkpoints preapproved by autodoc/autoloop, not individually human-accepted. |
| DOCS-009 | Unchecked and outside the selected loop stop. Whole-package QC and main-documentation integration require explicit advance. |

Material unresolved decisions: none at the initial approved scope. TypeScript/Canvas, fixed gravity, uniform draws, focus suspension and the stated non-goals are agent recommendations, not additional requirements supplied by the user. A fresh read-only builder check found no substantive blocker to presenting the baseline and cross-scope proposals; corrected preparation commentary in PROJECT and the publication-state wording here before submission. Do not claim human confirmation or adopt future-scope requirements early.

## Initialization and current reset

- Original presence: all four files existed: AI_DISCLOSURE.md, SDD-MANAGER.md, AGENTS.md and SKILL_DISCOVERY_PROTOCOL.md. No copies or changes to them.
- Preservation verified against original Git objects at `86aa03f573df4268fa32449523a253e9b9a217ff`. Existing disclosure and usage notice retained; they satisfy project attribution without replacement. README links follow with PROJECT.
- All-present initialization permits continuation without a mixed-state approval pause.
- Manual provenance: default enabled. Two bundled assets copied byte for byte on design-docs/ai-logs, seed `552a7390707543ecaf9c1f44308a0956f00b82c5`, based on main `86aa03f573df4268fa32449523a253e9b9a217ff`. Explicit two-parent main merge `706743cf9bec6cf37d9fd52293f7d69f32adc25c`; seed and merge pushed and exactly read back with confirmed process cleanup. No transcript created.
- Tracking: native sdd-docs-main milestone 1 and DOCS-001–009 issues 1–9 created from the bundled definitions before repository writes. Initial aggregate readback was incomplete; a fresh complete listing verified all nine exact titles, bodies, open states and milestone associations before writes. No duplicate objects were created.
- Credential: supplied repository token saved only after projection verification, with effective ignored/untracked status established; value excluded from evidence.
- Current reset: none. No prior-pass delivery ranges or acceptance evidence exist.

## Pending transitions

Publish/read back the current DOCS-005 completion and close/read back issue 5; then continue to DOCS-006. Git/provider observations settle these effects; earlier pass evidence is historical. Milestone 1 and DOCS-009 remain open.

## Pass 1 document evidence

- DOCS-002: `PROJECT.md` SHA-256 `2f71599a56c88431b4927d7ee6d6fbc8d1cf243324ae38e60feb6b02cfaf5f53`. Human approved the published PROJECT and presented cross-scope choices on 2026-10-24. Exact approved PROJECT bytes retained. Future checkpoints use autodoc/autoloop preapproval; no product implementation is authorized. Completion commit requires push/readback, then issue 2 closure/readback before progression.
- DOCS-003: `ARCHITECTURE.md` SHA-256 `4219974a10108cf2878a1c8b19709cc96619e3aa46cfbe06e884c33f739d8ee2`. Autodoc preapproval following initial human acceptance. Design review checked one state owner, inward dependencies, injected time/randomness, focus/restart cleanup, current-only scope and browser-policy boundary; no unresolved structural blocker. Completion commit requires push/readback, then issue 3 closure/readback before progression.
- DOCS-004: `DECOMPOSITION.md` SHA-256 `80397b8fc86379f3d072e716c17e3bb522247bad76ed9e079dd932c6aff4b9a8`. Autodoc preapproval. Component review checked single ownership of active/settled/time state, interface consumers, directed dependencies, rejection semantics and deterministic verification seams against PROJECT and ARCHITECTURE; no blocker. Completion commit requires push/readback, then issue 4 closure/readback before progression.
- DOCS-005: `SPEC.md` SHA-256 `f486ee7d00b4940703ff84cb3588b9206b791a3e8c976f7d06991d7c2bff629f`. Autodoc preapproval. SPEC/design QC is Ready in SPEC-REVIEW-REPORT.md with exact source hashes and R01-R07 coverage; baseline geometry, timing, rejection, lifecycle and browser acceptance contracts are assessable. No application checks claimed. Completion commit requires push/readback, then issue 5 closure/readback before progression.
- DOCS-006: `PLAN.md` SHA-256 `6848343264d64af5f6332af19ebaa39208b2057e710c521ae9145b1e12330a27`. Autodoc preapproval. PLAN strategy QC is Ready against R01-R07; two cohesive delivery milestones are justified without padding plus one dedicated phase review milestone. TASKS is held until combined PLAN/layout review. Completion commit requires push/readback, then issue 6 closure/readback before progression.
- DOCS-007: `layout.md` SHA-256 `371594330527d54c3268062f473afa13d4e1605576f1eb407aab240e98f02efa`. Autodoc preapproval. Combined PLAN/layout QC is Ready; every logical owner maps to intended source/tests, generated artifacts are isolated, and AGENTS navigation reflects actual documents without claiming executable setup commands. Completion commit requires push/readback, then issue 7 closure/readback before progression.

## Pass 1 delivery range

Phase 1, milestones 1.1–1.3, T-001–T-009. Protected PLAN block SHA-256 `a856ce52c8f032af791638a949f308a7697e374ccd0cd9b6e4c708021dcfcea3`; TASKS block SHA-256 `df447fe9f00fdc3855f09e7252959e42f9c4f4467648c1082090497d94fb2599`. Compare exact retained blocks before later planning/task publication.
- DOCS-008: `TASKS.md` SHA-256 `18aed12b67ce8dce9a7f9d4736527a7dfd7ecc9413798b4403e471744ef0f6f2`; `HANDOFF.md` SHA-256 `bea087f625c93644c30ac99f0b2369e1628e56104f8fa0f5e4b46f186fa72172`. Autodoc preapproval. TASKS conformance is Ready: nine unique unchecked tasks, six delivery tasks and three dedicated review tasks, complete R01-R07/PLAN coverage, feasible dependencies and justified counts. Phase 1/T-001-T-009 becomes protected; after closure reset for predefined Pass 2, without DOCS-009. Completion commit requires push/readback, then issue 8 closure/readback before progression.

## Reset into Pass 2

Prior Pass 1 DOCS-008 checkpoint `b8ad868711a4dd4ef4f95b95c8333fef57e107c9` was published/read back and issue 8 closed. Initial PROJECT approval and autodoc/autoloop authorize this next predefined scope and reset. All existing product documents and protected phase/task blocks are retained. DOCS-001 remains complete; DOCS-002–009 are unchecked. Publish this reset, reopen/read back the same issues 2–9, and verify milestone 1 open before document revision. No package QC, integration or implementation is selected.

## Pass 2 decisions

Inherited timing/hold/source decisions are accepted at initial review. Delegated detail: wall kicks use candidates (0,0), (-1,0), (1,0), (-2,0), (2,0), (0,-1), (0,-2) in that order for non-O pieces; this meets bounded wall-kick scope without extra rotation-system requirements. Special action key repeats are ignored. Delivery appends Phase 2 piece supply/hold and Phase 3 rotation/landing controls, separating two independently usable increments. Phase 1/T-001–T-009 stays unchanged. Reset commit and issue reopening were read back before this revision. Later documents use scoped autodoc preapproval.

## Pass 2 document evidence

- DOCS-002: `PROJECT.md` SHA-256 `6f777bc9067f539f9839de43e58447eff555d015e8ab6bbd275b2edd65746768`. Autoloop PROJECT preapproval under initial human approval. Cumulative scope includes only the baseline and gameplay enhancements; future visibility, penetrator and configuration requirements remain in the scope record. Completion commit requires push/readback, then issue 2 closure/readback before progression.
- DOCS-003: `ARCHITECTURE.md` SHA-256 `d479065413a80ddf85ada6e22750e9285af9f73b03415cff22ac628a902a9edd`. Autodoc preapproval. Reviewed source/hold consumption ownership, one landing calculation and single deadline owner against current PROJECT; adapters remain outside core and Phase 1 design invariants hold. Completion commit requires push/readback, then issue 3 closure/readback before progression.
- DOCS-004: `DECOMPOSITION.md` SHA-256 `b9325219ce1fefe7cf4b22cc6a931156d6a07103a0b02ece629b17f1a73c13b5`. Autodoc preapproval. Reviewed bag/hold/projector/deadline interfaces, test seams and consumption/lifecycle failure paths; all current capabilities have one logical owner. Completion commit requires push/readback, then issue 4 closure/readback before progression.
- DOCS-005: `SPEC.md` SHA-256 `cb7fa312af534a7cb173f86e959de4071f6bc6465a4d92ef5e05a66feeffd051`. Autodoc preapproval. Current SPEC/design QC Ready: R01-R09 coverage, exact hold/source consumption and shared landing/deadline behavior with assessable edge cases. Completion commit requires push/readback, then issue 5 closure/readback before progression.
- DOCS-006: `PLAN.md` SHA-256 `212f2a0ea46d846b38c246df814702ed4e672f50d49f001cc14507953ce85417`. Autodoc preapproval. PLAN strategy QC Ready; append Phases 2-3/milestones 2.1-2.2 and 3.1-3.3. Strict prior PLAN prefix verified; narrow phase/milestone counts justified. Combined placement still pending. Completion commit requires push/readback, then issue 6 closure/readback before progression.
- DOCS-007: `layout.md` SHA-256 `cf4f0ebfe80dffe8bf3e1f4a9209fef785df0ae0a319a0a756796c2f5d33d313`. Autodoc preapproval. Combined PLAN/layout QC Ready for bag/hold, kick/projector and timing owners; all current component paths/test seams covered. Completion commit requires push/readback, then issue 7 closure/readback before progression.

## Pass 2 delivery ranges

Appended Phase 2 / T-010–T-013 and Phase 3 / T-014–T-021. Phase 1 / T-001–T-009 retained byte for byte; exact prefix comparison runs before PLAN/TASKS publication. Inherited cross-scope decisions remain active.
- DOCS-008: `TASKS.md` SHA-256 `8ab25d5e54e0721e0a8ad286d3aff28271eed205cc41389fe8ca93898d8b1725`; `HANDOFF.md` SHA-256 `0a3742451dc462cb096c57b848598a205ad71706e4f18240332fb2e9c42d4762`. Autodoc preapproval. TASKS QC Ready with 21 unique unchecked tasks, new R08-R09 coverage and exact protected prior prefix. After issue closure, reset for predefined Pass 3; DOCS-009 remains unchecked. Completion commit requires push/readback, then issue 8 closure/readback before progression.

## Reset into Pass 3

Prior Pass 2 DOCS-008 checkpoint `b57c538f18118fd9dc5c1953fd1596a18f370809` was published/read back and issue 8 closed. Initial PROJECT approval and autodoc/autoloop authorize this next predefined scope and reset. All existing product documents and protected phase/task blocks are retained. DOCS-001 remains complete; DOCS-002–009 are unchecked. Publish this reset, reopen/read back the same issues 2–9, and verify milestone 1 open before document revision. No package QC, integration or implementation is selected.

## Pass 3 decisions

Carry forward the initial accepted midline decision: dynamic ghost visible only if its minimum occupied projected row is at least 10. A projection straddling the midline is hidden. This is a view policy over the shared landing result, without collision, drop or timer changes. Append Phase 4 / T-022–T-025; preserve Phases 1–3 unchanged. Reset/reopening were verified before authoring.

## Pass 3 document evidence

- DOCS-002: `PROJECT.md` SHA-256 `2346cb6a20032b986e79414e88b13b7a8d2727332a1dfa2d0cba8208c2c9f163`. Autoloop PROJECT preapproval. Scope adds only conditional ghost visibility, using the initially approved all-cells-in-lower-half interpretation; no configuration control is introduced. Completion commit requires push/readback, then issue 2 closure/readback before progression.
- DOCS-003: `ARCHITECTURE.md` SHA-256 `88b08aecbe4524c3cda237b8596fa571820be70d4f3276dd69d030ef00ba2964`. Autodoc preapproval. Reviewed visibility as a view decision over the shared core projection; hidden ghosts cannot change landing or timing. Clarified reducer ownership of the hold latch without changing the accepted state boundary. Completion commit requires push/readback, then issue 3 closure/readback before progression.
- DOCS-004: `DECOMPOSITION.md` SHA-256 `1d7a32bd1cf6c96d07764e6d61392e402f78a507dba8776314b50019a333548b`. Autodoc preapproval. Reviewed predicate inputs/output and view consumer, exact occupied-cell basis and unchanged projector/state responsibilities. Completion commit requires push/readback, then issue 4 closure/readback before progression.
- DOCS-005: `SPEC.md` SHA-256 `ec7b4c0d7d96172e1a54dd45675fc76ef9fd701ef96d0ef71bc00bc8cce52c9d`. Autodoc preapproval. SPEC/design QC Ready for R01-R10; threshold/straddling, redraw and hard-drop independence are precise and trace to the predicate/view seam. Completion commit requires push/readback, then issue 5 closure/readback before progression.
