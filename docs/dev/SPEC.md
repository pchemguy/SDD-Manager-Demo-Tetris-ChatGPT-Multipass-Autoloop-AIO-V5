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

Start/restart creates an empty board, score zero and one active piece. Every source draw independently chooses one of the seven kinds with equal probability. A test source may supply a known sequence. A draw is consumed only when a piece is required, never by rendering. If the spawn pose is blocked, set game over without replacing settled cells or awarding points. Game-over state has no controllable active piece.

## R03 — Movement and controls

Left/Right move one column; Down attempts one row downward. Up or X rotates clockwise; Z rotates counterclockwise. Enter restarts when the game surface has focus. A visible HTML restart button performs the same transition. Recognized game keys prevent page scrolling only while the game surface has focus; focused HTML controls retain native keyboard behavior.

Each keydown, including native repeat, supplies one action. There is no independent repeat timer. Reject out-of-bounds or overlapping moves/rotations without changing state or consuming a source draw. Rotation uses only the current origin, without position adjustment. Actions except restart are ignored during game over or suspension. Releasing focus clears transient input state.

## R04 — Gravity and locking

Gravity attempts downward movement every 1000 ms of active time. A successful downward attempt changes y by one. An attempted downward move that is blocked locks the current valid pose immediately; failed horizontal moves or rotations do not lock. Locking inserts all four cells atomically, clears rows and awards score, then requests the next piece. Reset the gravity accumulator when a fresh active piece spawns so an old remainder cannot immediately move it.

Gravity and user actions are serialized. A clock advance containing several due intervals processes them in order; stop at game over. Backward or nonfinite time samples are ignored. Focus loss or hidden page suspends elapsed active time; resumption resets the clock baseline, with no background catch-up. Restart clears all timing state.

## R05 — Clearing and scoring

After lock, identify all completely occupied rows, remove them simultaneously, preserve the order of surviving rows and prepend empty rows. Award 0, 100, 300, 500 or 800 points for clearing 0, 1, 2, 3 or 4 rows in that lock. A rejected action or spawn awards nothing. Score is the nonnegative integer sum since restart; gravity interval stays fixed. Board owns compaction; reducer owns score.

## R06 — Status, rendering and lifecycle

Render settled cells and active cells at their grid positions, with a visible boundary/grid and distinct nonwhite kind colors. HTML shows score, playing/suspended/game-over status, controls and restart. Canvas is keyboard-focusable and has an accessible label; buttons/status use native HTML semantics. This does not require a screen-reader equivalent of spatial play or touch controls.

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
