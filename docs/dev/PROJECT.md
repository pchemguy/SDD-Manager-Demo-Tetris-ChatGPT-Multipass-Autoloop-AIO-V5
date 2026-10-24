# Browser Tetris

## Purpose and users

Build a lightweight, single-player Tetris game for people playing with a keyboard in a desktop browser. The intended outcome is a complete, understandable game that can be built as static files and served without an application backend.

## Intended experience

The player moves and rotates falling tetrominoes on a 10-column, 20-row visible board. Gravity advances the active piece. When its selected locking policy is satisfied, the piece locks, completed horizontal lines clear, and the player earns points. A new piece enters play; inability to spawn ends the game. Restart begins a fresh game with an empty board and zero score.

The interface presents the board, current score, controls and game status clearly. Keyboard input controls horizontal movement, downward movement and rotation. Restart is available through a visible HTML control and the keyboard. Losing page focus suspends play so returning to the page does not apply a burst of missed gravity updates.

## Principal scope

- All seven tetrominoes, supplied by shuffled seven-piece bags.
- Grid-aligned movement and rotation with wall kicks, rejecting placements outside the board or overlapping settled cells.
- Fixed-interval gravity, with a default full grounded-interval lock and selectable failed-down locking; hard drop can follow that policy or lock immediately.
- Hold once per locked-piece cycle, a landing ghost hidden when its projection reaches into the upper half, and keyboard hard drop.
- Earn up to three penetrator bonuses from four-line clears. A bonus activates a white current piece that traverses occupied cells until locking at a legal nonoverlapping position; hold is disabled while active. Ghost and hard drop share its deepest valid landing.
- A side panel with six session-only game settings and an optional next-piece preview. Ghost visibility supports hide, dynamic and show; hard drop, next preview and hold can be enabled or disabled. Reload starts with defaults; restart retains current settings.
- Simultaneous removal of complete rows, downward compaction and line-clear scoring.
- Spawn-based game over and repeatable restart.
- A readable board with nonwhite regular-piece colors and white penetrator cells and accessible HTML status and controls.

## Technical direction and quality

The proposed implementation uses TypeScript, an HTML Canvas board and ordinary HTML controls. Game rules should be testable independently of rendering and browser timing. Avoid a UI framework or server unless a demonstrated requirement justifies one.

Chromium is the primary verification target; Playwright-managed Firefox is secondary. Headless interaction and screenshots are sufficient for the intended browser checks. Follow the repository's [browser development policy](../../BROWSER_DEV_POLICY.md) for isolated provisioning, diagnostics and execution evidence.

The first playable release is planned as v0.1.0. A release requires implemented functionality, successful verification and integration into the default branch.

## Boundaries and assumptions

Multiplayer, backend services, online leaderboards and mobile touch controls are outside the intended scope. The browser must support Canvas, standard keyboard events and the generated JavaScript. Score values, input mappings, rotation details and objective acceptance cases belong in the specification.
