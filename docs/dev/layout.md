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
