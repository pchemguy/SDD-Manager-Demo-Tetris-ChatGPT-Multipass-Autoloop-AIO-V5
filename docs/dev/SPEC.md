# System specification

## Scope and ownership

The game provides the experience in [PROJECT](PROJECT.md) through the blocks in [ARCHITECTURE](ARCHITECTURE.md) and responsibilities in [DECOMPOSITION](DECOMPOSITION.md). Requirements below define intended behavior, not implemented results. The static game has no server, account or network-dependent gameplay.

## R01 — Board and pieces

The visible board has 10 columns and 20 rows. Coordinates are zero-based, x increasing right and y increasing downward. Settled cells are empty or contain a tetromino kind/color. An active pose is separate from settled cells. Every accepted occupied cell is in bounds; ordinary pieces never overlap settled cells.

Kinds and spawn masks use `.` for empty and `X` for occupied; `/` separates rows. The enclosing square size fixes the rotation origin.

| Kind | Square size | Spawn mask |
| --- | --- | --- |
| I | 4 | `..../XXXX/..../....` |
| O | 2 | `XX/XX` |
| J | 3 | `X../XXX/...` |
| L | 3 | `..X/XXX/...` |
| S | 3 | `.XX/XX./...` |
| T | 3 | `.X./XXX/...` |
| Z | 3 | `XX./.XX/...` |

Spawn orientation is 0, with square origin x = floor((10 - square size) / 2), y = 0. There are no hidden rows. A clockwise rotation maps each square-local cell (x,y) to (size - 1 - y,x); counterclockwise is its inverse. O rotation leaves its occupied cells unchanged. The orientation sequence is 0, R, 2, L, 0. Geometry owns masks; board owns settled-cell validity.

## R02 — Spawn and randomness

Start/restart creates an empty board, score zero and one active piece. The classic source strategy independently draws each of the seven kinds with equal probability. Production uses the seven-bag strategy in R08. A test source may supply a known sequence. A draw is consumed only when a piece is required, never by rendering. If the spawn pose is blocked, set game over without replacing settled cells or awarding points. Game-over state has no controllable active piece.

## R03 — Movement and controls

Left/Right move one column; Down attempts one row downward. Up or X rotates clockwise; Z rotates counterclockwise. Enter restarts when the game surface has focus. A visible HTML restart button performs the same transition. Recognized game keys prevent page scrolling only while the game surface has focus; focused HTML controls retain native keyboard behavior.

Movement/rotation keydowns, including native repeat, supply one action. C and Space are non-repeating actions: ignore their native-repeat keydowns. There is no independent repeat timer. Reject out-of-bounds or overlapping moves/rotations without changing state or consuming a source draw. The classic rotation strategy uses only the current origin; production uses the kick resolver in R09. Actions except restart are ignored during game over or suspension. Releasing focus clears transient input state.

## R04 — Gravity and locking

Gravity attempts downward movement every 1000 ms of active time. A successful downward attempt changes y by one. Under the classic failed-down strategy, a blocked downward attempt locks the valid pose immediately. Production uses the full-interval strategy in R09. Failed horizontal moves or rotations do not lock. Locking inserts all four cells atomically, clears rows and awards score, then requests the next piece. Reset the gravity accumulator when a fresh active piece spawns so an old remainder cannot immediately move it.

Gravity and user actions are serialized. A clock advance containing several due intervals processes them in order; stop at game over. Backward or nonfinite time samples are ignored. Focus loss or hidden page suspends elapsed active time; resumption resets the clock baseline, with no background catch-up. Restart clears all timing state.

## R05 — Clearing and scoring

After lock, identify all completely occupied rows, remove them simultaneously, preserve the order of surviving rows and prepend empty rows. Award 0, 100, 300, 500 or 800 points for clearing 0, 1, 2, 3 or 4 rows in that lock. A rejected action or spawn awards nothing. Score is the nonnegative integer sum since restart; gravity interval stays fixed. Board owns compaction; reducer owns score.

## R06 — Status, rendering and lifecycle

Render settled cells and active cells at their grid positions, with a visible boundary/grid and nonwhite ordinary kind colors and the white penetrator appearance in R11. HTML shows score, playing/suspended/game-over status, controls and restart. Canvas is keyboard-focusable and has an accessible label; buttons/status use native HTML semantics. This does not require a screen-reader equivalent of spatial play or touch controls.

Restart is repeatable from playing, suspended or game over: clear board and score, draw a fresh piece and reset timing, while respecting current page focus/visibility. Mount creates one input-listener set and frame loop. Restart does not duplicate them. Dispose cancels owned callbacks/listeners and is safe when repeated. Failed Canvas/context initialization displays a startup error in HTML and starts no running session.

## R07 — Delivery and browser qualities

Build a static asset distribution that operates after serving it over HTTP without backend calls. TypeScript checking, rule/session checks and production build must succeed for acceptance. Chromium is primary and Playwright-managed Firefox secondary, using the [central browser policy](../../BROWSER_DEV_POLICY.md) for isolated provisioning, diagnostics, headless execution, interaction, rendering and teardown evidence. Do not infer application acceptance from browser installation or launch alone. Preserve Windows CMD compatibility under the root shell policy on Windows hosts.

## Acceptance scenarios

| Requirement | Observable acceptance |
| --- | --- |
| R01 | Every kind has four cells; four clockwise turns restore the original mask; O is unchanged; wall/floor/occupied-cell rejection preserves the prior board/pose. |
| R02 | A controlled sequence produces exactly that spawn order; blocked spawn ends play without board mutation; source boundary values select legal kinds. |
| R03–R04 | Keyboard movement/rotation appears on Canvas; at 999 ms no gravity step, at 1000 ms one step; blocked down locks once; failed side/rotation does not lock; new spawn gets a fresh interval. |
| R04, R06 | Hide/blur then resume after several seconds causes no missed-time burst; repeated restart leaves one frame loop and listener set; disposed sessions ignore stale callbacks. |
| R05 | Fixtures clearing 1–4 rows yield exact compacted boards and 100/300/500/800 points; two separate locks accumulate once each; restart zeros score. |
| R06–R07 | In both browsers, start, move, rotate, clear a line, reach game over and restart through real app inputs; inspect representative Canvas pixels/screenshots and HTML status; verify repeated isolated context/page cycles and teardown. |

Check selection may combine scenarios where one observation establishes several contracts. Implementation evidence must distinguish simulated rule transitions from actual browser interaction and rendering.

## R08 — Seven-bag supply and hold

Production draws from a uniformly shuffled permutation containing each kind once, then a fresh bag. Use injected random draws for deterministic checks; a Fisher–Yates permutation with j = floor(u * (i + 1)) is acceptable. The classic uniform source remains a core strategy without a UI selector. Restart clears bag/cursor and hold, starts a fresh bag and resets the hold allowance.

C holds an ordinary active kind at most once between locks; R11 prohibits hold while penetration is active. Empty hold stores it and consumes one next source kind; occupied hold swaps kinds without consuming a draw. Incoming pieces use the R01 spawn pose, orientation 0 and fresh gravity/grounding timing. The used flag stays set until lock, including after an empty-slot hold. Denied repeated hold is a complete no-op. A blocked incoming spawn ends the game without score or settled-board changes; the accepted hold-slot update remains. Show the stored kind, or an empty hold indicator, in HTML/Canvas side presentation.

Acceptance: two complete bags each contain all kinds; hold-empty consumes one draw, hold-swap consumes none, repeat is denied and the next lock restores allowance. Restart resets bag/hold. Blocked held spawn reaches game over. Observe C and the hold display through the actual app in both browsers.

## R09 — Rotation, landing ghost, hard drop and interval lock

For an ordinary non-O rotation, test offsets (0,0), (-1,0), (1,0), (-2,0), (2,0), (0,-1), (0,-2), in that order, relative to the original origin; accept the first wholly in-bounds, nonoverlapping rotated pose. Offsets use R01 coordinates. If none is legal, retain the exact prior state. O does not translate. This candidate order applies equally to clockwise/counterclockwise turns and I/J/L/S/T/Z.

Ordinary landing is the deepest pose reachable through successive legal downward steps from the current pose, stopping before the first blocked step. Draw its occupied cells as a visually distinct translucent/outlined ghost subject to R10, without modifying the board. Suppress the ghost when it coincides with the active pose, in game over or without an active piece. Space moves the active piece to that exact target; it does not award drop points or consume a source item until lock.

Production lock delay is 1000 ms of active time from first becoming grounded. Grounded means the next ordinary downward step would fail. Establish grounding immediately after spawn, movement, rotation or drop, not at the next gravity tick. At deadline lock once even without a failed-down action. A failed downward attempt before deadline is a no-op. Successful movement/rotation while still grounded and repeated/zero-distance hard drops retain the original deadline. Leaving support cancels it; landing again starts a fresh interval. Due locks are processed before an action stamped at the same active timestamp. Suspension preserves elapsed active time; restart and incoming-piece changes clear the outgoing deadline.

The classic failed-down strategy remains a core policy for its scoped behavior; it is not exposed by a setting here. Ghost and hard drop have no independent collision rules.

Acceptance: wall/floor fixtures prove candidate order and rejection; ghost equals hard-drop target; a grounded piece survives 999 ms and locks at 1000 ms. Grounded shifts/drop cannot extend the deadline; leaving support and regrounding can start a new one. Hard drop midway through a gravity cycle still gets a full grounded interval. Verify real input, ghost rendering, hold and hard-drop-to-lock in both browsers.

## R10 — Dynamic ghost visibility

For the 20-row board, the lower half is rows 10–19 inclusive. Show the ghost only when every projected occupied cell has y >= 10, in addition to the existing active/playing/noncoincident conditions. Equivalently, its minimum occupied row is at least 10. Any projected cell at row 9 or above hides the entire ghost; do not clip it or use its center, square origin or active-piece height.

Recompute visibility from the current landing result after each relevant state change. Movement or rotation can hide or restore it immediately. Visibility never changes the landing result, hard-drop destination, board, draw order, hold allowance or deadline.

Acceptance: projected minimum y values 9 and 10 produce hidden and visible respectively; a four-cell projection spanning rows 9 and 10 is hidden. Move/rotate between fixtures and observe hide/restore in both browsers while hard drop still reaches the same computed landing. Existing coincident/game-over suppression remains applicable.

## R11 — Penetrator mode and bonus bank

The bank starts at zero and holds 0–3 integer bonuses. A lock clearing exactly four rows awards one bonus, capped at three, regardless of whether that piece was ordinary or penetrator. Other clears award none. Restart resets the bank and active mode. Each event is processed once with the lock/clear/score transaction; repeated frames cannot award again.

B, on a non-repeating keydown with game focus, activates the current ordinary piece only while playing and a bonus is available. Validate a legal terminal at/below its current pose before atomically spending one bonus and setting penetrator mode. Absent bank, already-active mode, suspended/game-over state or invalid terminal rejects activation without spending or any state change. Mode cannot be toggled off and lasts through this piece's lock. Hold while active is a complete no-op: no slot/source/allowance/deadline changes.

For a penetrator pose, all four transit cells must be in bounds; settled-cell overlap is permitted and never deletes or changes the underlying cells. A terminal candidate uses the same x/orientation, an integer y at or below the current origin, all occupied cells in bounds and no overlap. Select the deepest such candidate over the entire downward range, including candidates beyond occupied barriers. Every accepted move/rotation must retain at least one candidate at or below its proposed pose. Otherwise reject it exactly. Penetrator rotation tests the R09 offsets in order using this transit/terminal eligibility, rather than ordinary occupancy rejection.

Gravity/soft down advances one row toward that terminal, including through overlaps; it stops at the terminal even if the next row is an overlapping transit pose. Terminal arrival establishes grounding under the selected locking policy. Hard drop moves directly to the same terminal and ghost previews exactly its occupied cells, subject to R10. Activation or motion that opens deeper travel cancels prior grounding; if it leaves an already-grounded terminal unchanged, retain the deadline. Lock can insert only a fully in-bounds, nonoverlapping pose. It never merges an overlapping transit pose or consumes/deletes underlying cells.

Render every active penetrator cell white. Its ghost uses white with the ghost outline/transparency treatment. On lock retain white appearance on its settled cells, which have ordinary fixed-cell collision behavior. Ordinary active/settled/held pieces never use white. The side panel shows exactly one lightning-bolt symbol per bonus, including zero symbols at zero; earning or spending updates the snapshot immediately. A numeric accessible label states the count.

Acceptance: a barrier fixture proves ordinary descent stops above it but penetrator ghost/drop find the deepest legal cavity below it. Exercise overlap transit, no-terminal rejection, boundary kicks, final legal insertion and preservation of crossed cells. At zero bank B is inert; at three a four-line clear stays at three; successful B spends once; repeat B and C are inert while active. A four-line clear by a penetrator can restore one spent bonus once. Verify activation mid-delay, unchanged-terminal deadline retention, deeper-travel cancellation, white rendering, 0–3 symbols and restart in both browsers.
