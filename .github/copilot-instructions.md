# Copilot instructions

Single-file space quiz app: everything (HTML, CSS, JS) lives in `index.html`. No build, no package manager, no tests, no linter, no server. Run it by opening the file in a browser (`file://` works).

## Constraints
- Keep it a single self-contained `index.html` with no external dependencies, fonts, or network calls.
- Layout is a narrow centered column (`main` max-width 480px); body text stays 14–16px sans-serif (system font stack).

## Architecture
- **Data:** `Q` array of `[question, [4 options], correctIndex]`. The quiz length (progress bar, "Question X of N", results) derives from `Q.length`, so adding/removing questions needs no other changes.
- **State:** module-level `i` (current question), `score`, `locked` (blocks double answers). `show()` renders a question, `pick(n)` grades it, `finish()` renders results, and the Play again handler resets state.
- **Two sections:** `#quiz` and `#result`, toggled with the `.hidden` class.

## Conventions
- **Theming:** all colors are CSS variables in `:root`, overridden in `@media (prefers-color-scheme:dark)`. Add new colors as variables in both blocks, never hard-coded.
- **Feedback:** correct = `.correct` (green, `pop` animation); wrong = `.wrong` (red, `shake` animation) and the right answer is always highlighted too.
- **Accessibility:** options are real `<button>`s; focus is moved deliberately (first option on new question, Next after answering, Play again on results). Preserve this when changing flow. The progress bar keeps `role="progressbar"` with `aria-valuenow` updated in `setProgress`, and feedback text is in an `aria-live` region. Animations are disabled under `prefers-reduced-motion`.
- Results emoji/message tiers are score-ratio thresholds in `finish()`.
