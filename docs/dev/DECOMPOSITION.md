# Component decomposition

The [architecture](ARCHITECTURE.md) separates rules, application and browser adapters. The following logical components own the intended behavior; they are not claims of existing implementation.

| Component | Owns / provides | Requires / excludes | Verification seam |
| --- | --- | --- | --- |
| Geometry | Seven shape definitions, orientation offsets and legal board coordinates. Provides occupied cells for a pose. | Plain piece/pose values; no score or input policy. | Enumerate rotations and boundary cells. |
| Board | Settled cells, overlap checks, atomic piece insertion, simultaneous full-row removal and compaction. | Geometry cells; never owns the active piece or mutates on rejected insertion. | Small fixture boards and exact resulting grids. |
| Piece source | Produces the next kind with independent uniform sampling. | Injected random numbers in [0,1); tests may provide an explicit sequence. No rendering. | Source sequences and range boundaries. |
| Game reducer | Session status, active pose, score, spawn, movement, rotation, failed-down lock and restart transitions. Provides state plus explicit events. | Board/geometry/source; no clocks or DOM. | Commands against deterministic states. |
| Session controller | One game instance, gravity accumulator, serial command delivery, suspension/resume and disposal. Provides command entry and read-only snapshot subscription. | Reducer and injected active-time clock; renderer receives snapshots only. | Controlled timestamps, subscriber observations and lifecycle counts. |
| Input adapter | Maps recognized keyboard and restart/focus/visibility events to commands, releases input on suspension. | Controller command entry; no direct board writes. | Actual browser events and focus changes. |
| View adapters | Canvas board/piece drawing and HTML score/status/instructions/restart. | Snapshot; no source consumption or state changes during rendering. | Snapshot rendering plus browser screenshot inspection. |

## State and interface boundaries

A game state contains a 20-row settled board, active kind/orientation/origin or no active piece, integer score, and playing/game-over status. A pose combines a piece kind, orientation and integer grid origin. The reducer owns all gameplay mutation and returns a complete state; consumers must not retain mutable internal board references. Geometry and board operations are reusable pure functions.

The controller accepts move-left, move-right, rotate-clockwise, rotate-counterclockwise, soft-down and restart actions. It also accepts elapsed active time and explicit suspend/resume signals. Invalid commands in game over are no-ops except restart. A failed downward attempt is distinct from a rejected horizontal move or rotation. Session initialization or restart obtains its first piece through the source exactly once.

The board inserts a piece only when all occupied cells are in bounds and empty. It computes all full rows before removing them, returning the cleared count to the reducer for scoring. The reducer then attempts the next spawn. The input adapter cannot award score, and the view cannot ask the piece source for a preview.

## Lifecycle and failure ownership

The controller creates one subscription and frame loop per mounted application. Restart resets game and timing state while reusing the same adapters. Suspension discards unconsumed held-key state and background elapsed time. Disposal is idempotent and prevents later commands or callbacks from mutating a destroyed session.

Rejected placement is an ordinary result, not an exception. Spawn obstruction is game over. Browser startup errors are handled by the application entry and shown in HTML. Source/clock doubles may fail tests explicitly; they do not require production recovery infrastructure.

## Dependency assessment

No component imports a browser adapter from the core. Geometry is shared because occupied-cell rules must agree between collision and drawing. Board owns settled data; reducer owns the active piece; controller owns elapsed time. These responsibilities give movement and row-clearing separate tests without an artificial class hierarchy. SPEC supplies precise geometry, input, timing and acceptance values.
