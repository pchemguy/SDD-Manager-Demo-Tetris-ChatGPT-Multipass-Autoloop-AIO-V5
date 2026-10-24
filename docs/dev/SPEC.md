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

Production draws from a uniformly shuffled permutation containing each kind once, then a fresh bag. Use injected random draws for deterministic checks; a Fisher–Yates permutation with j = floor(u * (i + 1)) is acceptable. The classic uniform source remains a core strategy without a UI selector. The source queue exposes the next unconsumed kind without consuming or reordering it for R12 preview. Restart clears bag/cursor and hold, starts a fresh bag and resets the hold allowance.

C holds an ordinary active kind at most once between locks; R11 prohibits hold while penetration is active. Empty hold stores it and consumes one next source kind; occupied hold swaps kinds without consuming a draw. Incoming pieces use the R01 spawn pose, orientation 0 and fresh gravity/grounding timing. The used flag stays set until lock, including after an empty-slot hold. Denied repeated hold is a complete no-op. A blocked incoming spawn ends the game without score or settled-board changes; the accepted hold-slot update remains. Show the stored kind, or an empty hold indicator, in HTML/Canvas side presentation.

Acceptance: two complete bags each contain all kinds; hold-empty consumes one draw, hold-swap consumes none, repeat is denied and the next lock restores allowance. Restart resets bag/hold. Blocked held spawn reaches game over. Observe C and the hold display through the actual app in both browsers.

## R09 — Rotation, landing ghost, hard drop and interval lock

For an ordinary non-O rotation, test offsets (0,0), (-1,0), (1,0), (-2,0), (2,0), (0,-1), (0,-2), in that order, relative to the original origin; accept the first wholly in-bounds, nonoverlapping rotated pose. Offsets use R01 coordinates. If none is legal, retain the exact prior state. O does not translate. This candidate order applies equally to clockwise/counterclockwise turns and I/J/L/S/T/Z.

Ordinary landing is the deepest pose reachable through successive legal downward steps from the current pose, stopping before the first blocked step. Draw its occupied cells as a visually distinct translucent/outlined ghost subject to R10, without modifying the board. Suppress the ghost when it coincides with the active pose, in game over or without an active piece. Space moves the active piece to that exact target; it does not award drop points or consume a source item until lock.

The default full-interval locking policy delays 1000 ms of active time from first becoming grounded; R12 selects the effective policy. Grounded means the next ordinary downward step would fail. Establish grounding immediately after spawn, movement, rotation or drop, not at the next gravity tick. At deadline lock once even without a failed-down action. A failed downward attempt before deadline is a no-op. Successful movement/rotation while still grounded and repeated/zero-distance hard drops retain the original deadline. Leaving support cancels it; landing again starts a fresh interval. Due locks are processed before an action stamped at the same active timestamp. Suspension preserves elapsed active time; restart and incoming-piece changes clear the outgoing deadline.

The failed-down strategy in R04 is also available through R12; its blocked-down attempt locks immediately at a valid terminal pose. Ghost and hard drop have no independent collision rules.

Acceptance: wall/floor fixtures prove candidate order and rejection; ghost equals hard-drop target; a grounded piece survives 999 ms and locks at 1000 ms. Grounded shifts/drop cannot extend the deadline; leaving support and regrounding can start a new one. Hard drop midway through a gravity cycle still gets a full grounded interval. Verify real input, ghost rendering, hold and hard-drop-to-lock in both browsers.

## R10 — Dynamic ghost visibility

For the 20-row board, the lower half is rows 10–19 inclusive. In dynamic visibility mode, show the ghost only when every projected occupied cell has y >= 10, in addition to the existing active/playing/noncoincident conditions. Equivalently, its minimum occupied row is at least 10. Any projected cell at row 9 or above hides the entire ghost; do not clip it or use its center, square origin or active-piece height.

Recompute visibility from the current landing result after each relevant state change. Movement or rotation can hide or restore it immediately. Visibility never changes the landing result, hard-drop destination, board, draw order, hold allowance or deadline.

Acceptance: projected minimum y values 9 and 10 produce hidden and visible respectively; a four-cell projection spanning rows 9 and 10 is hidden. Move/rotate between fixtures and observe hide/restore in both browsers while hard drop still reaches the same computed landing. Existing coincident/game-over suppression remains applicable.

## R11 — Penetrator mode and bonus bank

The bank starts at zero and holds 0–3 integer bonuses. A lock clearing exactly four rows awards one bonus, capped at three, regardless of whether that piece was ordinary or penetrator. Other clears award none. Restart resets the bank and active mode. Each event is processed once with the lock/clear/score transaction; repeated frames cannot award again.

B, on a non-repeating keydown with game focus, activates the current ordinary piece only while playing and a bonus is available. Validate a legal terminal at/below its current pose before atomically spending one bonus and setting penetrator mode. Absent bank, already-active mode, suspended/game-over state or invalid terminal rejects activation without spending or any state change. Mode cannot be toggled off and lasts through this piece's lock. Hold while active is a complete no-op: no slot/source/allowance/deadline changes.

For a penetrator pose, all four transit cells must be in bounds; settled-cell overlap is permitted and never deletes or changes the underlying cells. A terminal candidate uses the same x/orientation, an integer y at or below the current origin, all occupied cells in bounds and no overlap. Select the deepest such candidate over the entire downward range, including candidates beyond occupied barriers. Every accepted move/rotation must retain at least one candidate at or below its proposed pose. Otherwise reject it exactly. Penetrator rotation tests the R09 offsets in order using this transit/terminal eligibility, rather than ordinary occupancy rejection.

Gravity/soft down advances one row toward that terminal, including through overlaps; it stops at the terminal even if the next row is an overlapping transit pose. Terminal arrival establishes grounding under the selected locking policy. Hard drop moves directly to the same terminal and ghost previews exactly its occupied cells, subject to R10. Activation or motion that opens deeper travel cancels prior grounding; if it leaves an already-grounded terminal unchanged, retain the deadline. Lock can insert only a fully in-bounds, nonoverlapping pose. It never merges an overlapping transit pose or consumes/deletes underlying cells.

Render every active penetrator cell white. Its ghost uses white with the ghost outline/transparency treatment. On lock retain white appearance on its settled cells, which have ordinary fixed-cell collision behavior. Ordinary active/settled/held pieces never use white. The side panel shows exactly one lightning-bolt symbol per bonus, including zero symbols at zero; earning or spending updates the snapshot immediately. A numeric accessible label states the count.

Acceptance: a barrier fixture proves ordinary descent stops above it but penetrator ghost/drop find the deepest legal cavity below it. Exercise overlap transit, no-terminal rejection, boundary kicks, final legal insertion and preservation of crossed cells. At zero bank B is inert; at three a four-line clear stays at three; successful B spends once; repeat B and C are inert while active. A four-line clear by a penetrator can restore one spent bonus once. Verify activation mid-delay, unchanged-terminal deadline retention, deeper-travel cancellation, white rendering, 0–3 symbols and restart in both browsers.

## R12 — Settings model and next preview

The side panel provides these labeled native controls. Reject invalid values independently without changing the prior valid model value. Programmatic invalid updates also return unchanged settings; no control event may produce a partially invalid model.

| Field | Allowed values | Default |
| --- | --- | --- |
| `ghost` | `hide`, `dynamic`, `show` | `dynamic` |
| `hardDrop` | boolean | true |
| `nextPreview` | boolean | true |
| `hardLock` | `general`, `immediate` | `general` |
| `lock` | `failed`, `interval` | `interval` |
| `hold` | boolean | true |

The mounted page owns the validated desired settings model. Page initialization and reload resolve its values under R13; game logic and controls do not directly access persistence. Restart retains the selected settings while resetting board, score, source, held kind, hold allowance and bonus bank. Each page has its own settings model.

Ghost mode `hide` suppresses it; `dynamic` applies R10; `show` permits it anywhere. All modes retain R09 active/playing/noncoincident suppression and the same normal/penetrator target. Ghost/preview visibility changes apply immediately. Preview shows exactly the next unconsumed kind; showing/hiding it cannot consume a draw, shuffle again or change the subsequent sequence. Empty hold consumes that same next kind; occupied hold swap leaves the preview unchanged.

`hardDrop=false` makes Space a complete no-op, with no target, timer or source change. `hold=false` makes C a complete no-op and retains the stored held kind/used allowance. Re-enabling hold restores availability only if the current cycle still permits it; active penetrator hold remains forbidden regardless of the setting. These availability choices apply immediately, without resetting an existing deadline.

Latch `lock` and `hardLock` from desired settings whenever a kind becomes the active piece, including empty/swap hold. Edits do not change an existing piece's effective policies or deadline. `lock=failed` uses R04 failed-down locking at the selected normal/penetrator terminal. `lock=interval` uses the R09 grounding deadline. `hardLock=general` repositions to the shared target and follows that general policy: in failed mode hard drop itself is not a failed downward attempt, so a later blocked down/gravity attempt locks. `hardLock=immediate` locks a valid target atomically on the hard-drop action, including zero-distance drop, then clears/awards/spawns exactly once. Disabling hard drop takes precedence over either hard-drop policy.

Controls remain usable while playing, suspended or game over, and show desired selections. Focused inputs/selects/buttons keep native navigation; game shortcuts are handled only by the game surface. A focus change follows the existing suspension rules rather than leaving stuck input.

Acceptance: exercise each default and allowed value, invalid updates, all four general/hard-lock combinations, mid-piece policy edits and incoming-piece latching. Toggle visibility/availability during ordinary/penetrator play without changing board, sequence or existing deadline. Verify retained held kind/allowance and preview draw counts. In both browsers change controls through native input, restart with selections retained, reload with R13 source resolution, and inspect the next preview and focus behavior. Confirm persistence is confined to the R13 adapters and does not change current-piece latching.

## R13 — Cookie and URL configuration persistence

All R12 fields participate independently. Query names are `ghost`, `hardDrop`, `nextPreview`, `hardLock`, `lock` and `hold`; cookie names are the corresponding query name prefixed with `tetris_`. Enum values are the exact case-sensitive R12 tokens. Booleans serialize as `1` or `0`; other spellings, empty strings and values outside the field's allowed set are invalid. Decode query/cookie values safely; malformed encodings must not crash initialization. Repeated occurrences of a known query name, even identical, are invalid for that field. Duplicate matching cookie names are invalid for that cookie field. Ignore unknown fields.

For each field choose: a valid explicitly supplied URL value; otherwise a valid saved cookie; otherwise its R12 default. Invalid URL input falls through without destroying a valid cookie. An invalid cookie falls through to default without affecting other fields. Resolve the complete model before first spawn so the first piece receives its chosen policies. Rendering/restart does not repeat source restoration or overwrite current selections.

For every valid explicit URL value, write that same value to its corresponding cookie during initialization, including when equal to the default. An omitted or invalid URL field generates no cookie write: retain its saved cookie rather than saving a default over it. Do not add omitted setting parameters to the URL merely because a cookie/default supplied the value.

Whenever a valid control change is accepted, update the live desired model, save that field's canonical cookie and set its canonical query parameter using replaceState without navigation or an added history entry. Consolidate duplicate occurrences of that changed field to one canonical value. Preserve other setting parameters, unrelated query values and the fragment. Attempt cookie and URL writes independently; failure of one does not suppress the other or roll back the model. Preserve the R12 immediate-availability and next-piece lock-policy rules; the URL represents desired settings, not an older active piece's latched policies.

Cookie writes use Path=/, Max-Age=31536000 and SameSite=Lax, with Secure on HTTPS. Values are individually encoded; no combined configuration blob is required. Cookie unavailability, expiry, malformed data or denied writes must not prevent play. If persistence fails, retain the live selection and provide a nondisruptive indication that settings/link updates could not be saved as applicable. Do not silently use local/session storage or any server. Opening an explicit valid URL still applies its settings when cookie writes fail.

Acceptance cases include:

- URL `ghost=dynamic` over cookie `hide` applies/saves `dynamic`, although it is the default.
- Omitted `ghost` with cookie `hide` applies `hide`, leaves the cookie unchanged and does not inject a URL default.
- Invalid/empty/duplicate URL `hardDrop` with valid cookie `0` selects false and does not overwrite that cookie.
- A mixed URL/cookie set resolves each of all six fields independently, including valid false values; invalid cookie plus omitted URL uses only that field's default.
- Changing one field updates only its cookie/query entry, retains unrelated values/hash and current game state, and survives reload/reopening through a fresh page/context with retained cookies.
- First-spawn effective policies reflect restored values; later edits retain current-piece latches. Restart retains current desired settings without another restore.
- Blocked cookies and simulated history failures preserve play, attempt the other write, and do not cause navigation, duplicated mounting or an uncaught exception.

Exercise pure resolver/codec tables plus actual cookies, native control changes, URL changes and reload in Chromium and Playwright-managed Firefox. A default-only or in-memory check cannot establish persistence acceptance.
