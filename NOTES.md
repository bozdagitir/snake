# NOTES

## Issues Found and Fixes Made

- **Issue:** Snake could pass through the wall and reappear on the other side instead of dying.
  **Request:** "If snake hits the wall game is over. Fix this."
  **Fix:** Removed the modulo wraparound on the head's position and added a wall-bounds check (`x`/`y` outside the grid) that ends the game, alongside the existing self-collision check.

- **Issue:** The "game over" message only appeared as text below the canvas, not in the middle of the game itself.
  **Request:** "When the game is over, can you print 'game over' in the middle of the game."
  **Fix:** Added a semi-transparent overlay drawn directly on the canvas that shows "GAME OVER" and the final score centered on the board when the game ends.

- **Issue:** The game needed more difficulty/challenge.
  **Request:** "Add one block in the size of the food close to each corner."
  **Fix:** Added four fixed obstacle blocks inset near each corner, matching the food's size, that end the game on collision and are excluded from food spawn locations. (This was later removed — see below.)

- **Issue:** The snake moved cell-to-cell in discrete jumps instead of sliding.
  **Request:** "The snake does not seem to move smoothly, can you fix it so it slides smoothly."
  **Fix:** Split the game loop into a fixed-interval logic step (movement, collisions, growth) and a per-animation-frame render step that interpolates each segment's drawn position between its previous cell and its current cell.

- **Issue:** The snake should get progressively harder to control over time.
  **Request:** "When the snake eats the food, it should get faster, so the game will be more difficult."
  **Fix:** Introduced a `stepMs` interval that decreases by a fixed amount each time food is eaten, down to a minimum floor so the game never becomes unplayable.

- **Issue:** After the smoothing fix, the snake's look/behavior changed in an unwanted way — the body appeared to move/shift oddly ("body parts should not move").
  **Request:** "Not exactly what I want, when it moves we should see each part exactly, the body parts should not move, can you fix this?" — followed later by "It looks like snake is moving square by square, its not sliding as I expected."
  **Fix:** First reverted to exact, non-interpolated grid-snapped rendering per the initial request. After clarifying (via a follow-up question) that continuous smooth gliding was actually wanted, reintroduced interpolation — this time correcting an indexing bug so each body segment slides from its own previous cell to its own new cell (matching by the same array index), instead of incorrectly sourcing from a neighboring segment's old position, which had caused the visible jumping/shifting artifact.

- **Issue:** Once sliding was implemented, the perceived pace felt off — first too fast, then too slow after smoothing.
  **Request:** A series of speed adjustments: "The game starts too fast can you make it a bit slower," then "Can you make it a little bit faster? Now its too slow."
  **Fix:** Tuned the `INITIAL_STEP_MS` constant iteratively (110 → 160 → 130ms) to land on a comfortable starting pace.

- **Issue:** The corner obstacles added earlier were no longer wanted.
  **Request:** "Remove the squares in each corner."
  **Fix:** Removed the obstacle array, its collision check, its rendering, its exclusion from food placement, and the now-unused obstacle color and hint text referencing it.
