# Orbit space quiz

## Running and verification

Open `index.html` directly with a `file://` URL in the integrated browser.
On macOS, `open index.html` opens it in the default browser. Keep direct-file
execution working: the application is a single HTML file with no server,
dependencies, build step, or external resource requests.

Verification is browser-based; there are no configured test or lint commands.
For a single-question smoke test, reload the page and select Mars on question 1:
the score should become 1/10, progress should become 1/10, the answer should turn
green, and Next should become enabled. Reload and select Venus to check red
shake feedback, the revealed correct answer, and an unchanged score of 0/10.

For a full run, the zero-based correct answer indices in question order are
`[1, 2, 3, 0, 2, 1, 3, 0, 2, 1]`. Recheck this key after changing question data.
Verify:

- Next cannot skip an unanswered question; repeated selections cannot score twice.
- Progress counts answered questions and reaches 10/10 before showing results.
- Results, accuracy, and answer review agree with the selected answers.
- Result reactions change at scores 4, 7, and 10; check both sides of each boundary.
- Restart resets score, progress, selections, question number, and the review disclosure.
- Native Tab, Shift+Tab, Enter, and Space navigation works; programmatic focus
  checks alone do not establish that real keystrokes work.
- A-D select answers, Enter advances after an answer, and held keys cannot skip
  questions. Shortcuts must not interfere with the theme button or form fields.
- The Keyboard controls disclosure starts collapsed on reload and remains
  available during the quiz and on results. Its open state persists through
  questions, results, and restart, independently of the answer review.
  Use real Enter and Space input to toggle it before and after answering and
  on results; verify score, progress, and question state do not change.
  Check held Enter on the guide and Next, and visible focus at 280px in both themes.
- Light/dark system preferences and the manual theme toggle work. Manual theme
  overrides last until reload; initial theme always follows the system.
- Reduced motion disables ambient, entrance, and feedback animation. Narrow
  layouts (including 280px) remain usable without horizontal scrolling.

## Architecture

`index.html` contains the entire application: semantic HTML for the quiz and
results, inline CSS, and a strict-mode classic script at the end of the body.
State lives only in memory and resets on reload.

The `questions` array is the content source. Each item has `category`, `question`,
four `answers`, a zero-based `correct` index, and `explanation`. Keep ten
questions unless explicitly asked to change the quiz length: introductory copy,
initial HTML counters, progress ARIA maximum, perfect-score threshold, and
percentage calculation also encode ten.

The rendering flow is `renderQuestion()` -> `selectAnswer()` -> `nextQuestion()` ->
`renderQuestion(true)` or `showResults()`. The state variables are `current`
(zero-based question index), `score`, `answered` (selection lock), and `selections`
(answer indices in question order). `updateProgress()` synchronizes the ten visual
segments, ARIA progress values, and score. Results derive their review from
`questions` and `selections`; there is no separate review data source.

## Implementation conventions

- Keep styles, scripts, system fonts, and CSS-drawn space artwork self-contained
  in `index.html`; do not introduce a framework, asset download, or package setup.
- Theme colors are semantic CSS custom properties in `:root`, overridden by
  `prefers-color-scheme: dark` and `data-theme` for the manual toggle. Keep
  dark-mode values in both overrides synchronized. The decorative expedition
  panel stays dark in both themes; interactive quiz surfaces follow the theme.
- The layout uses a 1320px maximum two-panel workspace, system sans-serif fonts,
  warm orange accents, and a single-column layout below 768px. Mobile artwork
  becomes a compact decorative illustration beside the introduction.
- CSS animations handle staggered question entrances, ambient planet movement,
  orbit travel, scoring and answer feedback. Animate transform and opacity only.
  Remove `.is-entering` after the last staggered answer, or on selection, so
  entrance animations do not override feedback or hover interactions.
- State classes `.correct`, `.wrong`, and `.unselected` drive answer feedback.
  Correct answers use green celebration feedback; wrong selections use red
  shake feedback while the correct option is also revealed. Reduced-motion
  preferences disable animations and transitions.
- Use `get(id)` for ID lookups. Dynamic question, answer, explanation, and review
  strings are inserted through `textContent`; `innerHTML` is used for controlled
  markup templates. Answer buttons are rebuilt with `replaceChildren()`.
- Preserve the `[hidden]` display rule, live score and feedback regions, progress
  ARIA values, answer-group label, and textual correctness indicators.
- Selection disables all answer buttons and focuses Next without scrolling.
  Advancing and restarting focus the question heading; results focus their
  heading. Those headings use `tabindex="-1"` for programmatic focus. A-D labels
  identify choices visually and match the keyboard shortcuts.
- Keep disclosures native: exclude summary targets from quiz shortcuts so Enter
  and Space retain their normal toggle behavior without advancing the quiz.
- Results include a score ring, per-question correctness strip, accuracy, and a
  detailed answer review. All derive from the same selections and questions.
