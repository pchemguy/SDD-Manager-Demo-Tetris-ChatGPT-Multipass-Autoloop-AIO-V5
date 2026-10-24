# Architecture

## System context

The [project brief](PROJECT.md) defines a static, keyboard-operated browser game. The browser owns one game session; no service or external account participates. A static asset host delivers the build and has no gameplay responsibility.

## Blocks and dependency direction

| Block | Responsibility | Dependencies |
| --- | --- | --- |
| Rules core | Board, tetromino geometry, legal poses, spawn, movement, locking, row clearing, score and game status. | Plain data and an injected piece source; no DOM, Canvas, storage or wall clock. |
| Session application | Own one core state, process ordered actions, advance active time and publish a render snapshot. | Rules core, injected clock/source and presentation ports. |
| Browser adapters | Translate keyboard/focus/button events, schedule animation frames and draw Canvas/HTML views. | Session commands and read-only snapshots; browser APIs remain here. |
| Build and verification | Compile static assets and exercise pure rules plus browser integration. | Public core/session seams and the built application. |

Browser events flow inward as commands; immutable snapshots flow outward. The core never imports an adapter. The application is the sole authority for state mutation, preventing renderer/input races. A small set of functions and typed records is sufficient; no framework, dependency-injection container or event bus is required.

## Session and time ownership

One application instance owns board, active piece, score, game status and elapsed active time. The piece source is injected so tests can reproduce a game without changing production randomness. The browser scheduler converts frame timestamps into elapsed active time; keyboard events become discrete actions. Events are processed serially, with each transition completing before rendering.

Focus loss or page hiding suspends active time and clears held-input state. Resumption establishes a new timestamp baseline, without simulating missed background time. Restart creates fresh core state and resets the scheduler baseline; it does not add event listeners or animation loops. Disposal cancels the owned frame request and listeners.

## Rendering and failure boundaries

Canvas draws board and pieces from snapshots; HTML owns score, instructions, restart and game-over status. Rendering cannot change collision or scoring. Regular tetromino colors exclude white. Keyboard handling is limited to recognized game actions, and focused HTML controls retain normal browser behavior.

An invalid gameplay action returns unchanged state rather than throwing. A blocked spawn produces game over. A missing Canvas context or failed application initialization produces a visible startup error rather than a misleading running game. Build/network errors are deployment failures, not gameplay state.

## Verification and tradeoffs

Pure rules accept deterministic boards and piece sequences; the application accepts a controlled clock. These seams support boundary checks without simulating browser frames. Browser checks establish actual keyboard delivery, Canvas output, status controls and restart behavior in the [required isolated environments](../../BROWSER_DEV_POLICY.md). Their evidence is separate from unit checks.

Canvas keeps grid drawing straightforward; HTML preserves native controls and status semantics. This does not promise a fully screen-reader-playable spatial board. DECOMPOSITION owns logical responsibilities and interface detail; SPEC owns observable contracts.

## Piece supply and landing policies

The source owns a shuffled bag and its cursor; hold state belongs to the reducer and consumes the source only for an empty hold slot. The controller owns the one-hold-per-cycle latch and time coordination through reducer state, with no duplicate view state. The view draws the held kind from a snapshot without drawing another source item.

A shared pure landing projection serves ghost and hard drop. Rotation tries a bounded deterministic candidate list through board validity. The controller owns grounding timestamps and deadlines; actions report whether the piece remains supported so the same deadline governs gravity, soft drop and hard drop. Rendering and input cannot reset it. Production selects seven-bag supply and interval locking; the classic source/locking strategies remain testable core policies for their scoped contracts.
