# Speed-Code

Speed-Code is a dependency-free, local two-player coding racing game. Players share one laptop and alternate turns answering the same Python questions. Correct answers advance the car; incorrect answers leave it in place and add time penalties. The lowest final adjusted time wins.

## Run the game

No installation, server, build tools, or internet connection are required.

1. Open `index.html` in any current desktop browser.
2. Enter both player names, choose 5, 10, or 15 rounds, and select **Start Race**.
3. On each ready screen, pass the laptop to the named player before pressing **Start Turn**.

You can also serve the folder with any simple static server, but this is optional. The application is specifically designed to work from a `file://` URL.

## How a match works

The flow is:

`Main menu → Player 1 ready/turn → handoff → Player 2 ready/turn → next round → results`

- A shuffled question sequence is selected at the start of every match.
- Both players receive the same question and option order for each round.
- Answers can be selected with the mouse or number keys 1–4.
- Every correct answer completes one checkpoint and advances that player's car.
- A correct answer triggers a turbo effect. An incorrect answer leaves the car at its current checkpoint and adds a 3-second penalty.
- Timing runs only from **Start Turn** until an answer is accepted. Menus, handoffs, feedback, and results are excluded.
- Each player has an independent timer based on `performance.now()`. The interface refreshes the visible time once per second, while scoring retains the full millisecond-precision measurement.

The scoring formula is:

```text
Final adjusted time = total active answer time + (incorrect answers × 3 seconds)
```

The lowest adjusted time wins. Adjusted times within 0.01 seconds are declared a tie.

## Project structure

- `index.html` — all markup, styling, question data, game state, timing, racing, rendering, sounds, and input handling.
- `README.md` — setup, architecture, and extension notes.

The JavaScript in `index.html` is separated into numbered sections so it can later be extracted into modules:

1. Game configuration
2. Question bank
3. Game state
4. Timer and scoring
5. Turn management
6. Racing mechanics
7. UI rendering
8. Event handling

## Adding questions

Add another object to `QUESTION_BANK` in `index.html`:

```js
{
  id: "python_037",
  language: "python",
  difficulty: "easy",
  category: "arithmetic",
  prompt: "What does the following code output?",
  code: "print(4 + 4)",
  options: ["6", "7", "8", "44"],
  correctIndex: 2,
  explanation: "The + operator adds the two numbers to make 8."
}
```

Requirements for each question:

- Use a unique `id`.
- Provide exactly four string options.
- Set `correctIndex` to the zero-based position of the one correct option.
- Keep `language` aligned with a supported language value.

The built-in validator skips malformed questions and duplicate IDs. Match setup also prevents selecting more questions than are available.

## Current scope and limitations

- Python, easy difficulty, multiple-choice questions, and one racing theme are implemented.
- The game is local and turn-based; no data persists after the page is closed.
- Sound uses lightweight synthesized Web Audio tones and depends on browser audio support.
- Random selection uses `Math.random()`, which is appropriate for casual play but not seeded or cryptographically fair.

## Suggested next iteration

Extract configuration, question data, state, and rendering into ES modules; add difficulty filters and a C++ question bank; add automated browser tests for complete match flows; then add track/car choices and persistent local statistics before considering online multiplayer or a backend.
