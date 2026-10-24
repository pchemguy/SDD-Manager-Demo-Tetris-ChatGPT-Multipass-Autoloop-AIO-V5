# Physical layout

## Ownership map

These are intended implementation paths. Only the documentation, policies and bundled agent packages exist at preparation time.

| Path | Owner and purpose | Verification home |
| --- | --- | --- |
| `src/core/types.ts`, `geometry.ts`, `board.ts` | Rules data, masks/rotation and settled-board operations. No browser imports. | `tests/unit/core/` geometry and board fixtures. |
| `src/core/source.ts`, `game.ts` | Uniform/injected piece source and reducer transitions. | `tests/unit/core/` source and game checks. |
| `src/app/session.ts` | Active time, one reducer instance, command serialization and lifecycle. | `tests/unit/app/` controlled clocks/subscribers. |
| `src/browser/input.ts`, `view.ts`, `main.ts` | Keyboard/focus, Canvas/HTML snapshots and startup/disposal composition. | `tests/browser/` real event, rendering and lifecycle cases. |
| `index.html`, `src/browser/styles.css` | Static shell, native controls and presentation. | Browser layout/focus/screenshot assertions. |
| `package.json`, lockfile, `tsconfig.json`, `vite.config.ts`, `playwright.config.ts` | Pinned build/test toolchain and browser projects; no product dependency on test runner. | Type checking, unit tests, served production build and both-browser suite. |
| `tests/fixtures/` | Deterministic board/piece fixtures through controlled test seams. | Shared assertions; exclude fixture controls from production UI. |
| `docs/dev/` | Current design/specification/strategy/tasks and adjacent QC reports. | Preparation owner review; no implementation report claims. |
| `docs/dev/reports/phases/<phase>/` | Implementation milestone/phase reports when executed. | Owning TASKS review unit. |
| `README.md`, `RELEASE-NOTES.md` | Actual play/build instructions and finalized release message when delivered. | Served-build and release-source verification. |

## Placement and generated artifacts

Core owns domain types; application depends inward on core; browser composes them. Tests consume public boundaries rather than import browser adapters into core. Task scopes may touch a few collaborating modules and their focused tests without requiring a file per requirement.

Keep `dist/`, runner output, screenshots/videos, local browser binaries/state and dependencies out of tracked source with explicit ignore rules when those paths are created. Browser binary/cache paths must be dedicated and absolute under the central browser policy. Do not use system/personal browser profiles. Retain selected acceptance images through the implementation report's evidence workflow, without committing generated browser installations.

Existing `.agents/` packages, root governing policies, disclosure/usage files and `docs/ai-logs/` remain separate from product code. Manual transcripts are historical data, outside routine validation. Preserve the ignored repository token convention. No modification of `/pyenv` is permitted.

The intended toolchain is TypeScript/Vite with unit checks and Playwright. T-001 establishes actual scripts and pins dependency versions; this document does not claim that commands have already been installed or verified. On Windows, follow the root CMD shell policy.

## Supply and landing ownership

`src/core/source.ts` owns bag/uniform strategies; `src/core/game.ts` owns hold state and actions. `src/core/rotation.ts` owns kick resolution, `src/core/landing.ts` owns projection, and `src/app/session.ts` owns grounding deadlines. Unit fixtures remain under core/app test homes; browser cases and held/ghost presentation remain under the existing browser adapters. These modules separate independent change/test responsibilities without duplicating board geometry or state.

## Ghost visibility ownership

`src/core/ghost.ts` owns the pure occupied-cell visibility predicate and `tests/unit/core/ghost.test.ts` owns threshold fixtures. The existing landing projector supplies cells; browser snapshot/view paths consume the boolean. `tests/browser/ghost.spec.ts` owns dynamic hide/restore and drop-independence interaction/rendering checks. No configuration or storage path is introduced.

## Penetrator ownership

`src/core/penetrator.ts` owns transit/terminal policy and focused cavity fixtures live under `tests/unit/core/`. Existing `game.ts` owns activation/bank/award transitions; `board.ts` validates final insertion and settled appearance. `session.ts` consumes policy grounding. Existing input/view adapters own B activation, white drawing and the count indicator. `tests/browser/penetrator.spec.ts` owns the served barrier/cavity, timing and screenshot scenarios. Fixtures use controlled test seams, not production cheat controls.

## Configuration ownership

`src/app/settings.ts` owns schema/defaults and the page model; `src/browser/settings.ts` owns labeled native controls. Existing session/game/drop/ghost paths consume the validated values and effective per-piece policies. Source peek remains in `src/core/source.ts`, with preview drawing in the existing view. Unit tests live under `tests/unit/app/` and source tests under core; `tests/browser/settings.spec.ts` owns control, focus, matrix and reload/restart scenarios. The settings model/form do not directly access persistence; dedicated browser adapters own those effects.

## Persistence ownership

`src/app/settings-codec.ts` owns canonical field codecs and pure resolution; `src/browser/cookies.ts` and `src/browser/url-settings.ts` own browser effects. Bootstrap/change orchestration stays in `src/browser/main.ts` and the existing settings adapter. Resolver tables live under `tests/unit/app/`, adapter failure checks under `tests/unit/browser/`, and actual cookie/URL/reload scenarios under `tests/browser/settings-persistence.spec.ts`. No storage string format leaks into core gameplay. Existing policies/packages/logs remain outside product ownership.
