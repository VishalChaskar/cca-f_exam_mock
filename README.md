# CCA Foundation Exam Quiz

A dark-themed practice quiz for the CCA Foundation exam. Single-page, no build step, no frameworks.

## Files

| File | Purpose |
|---|---|
| `index.html` | Complete app — markup, styles, and logic in one file |
| `ccaf_questions_consolidated.json` | Question bank, loaded at runtime |
| `README.md` | This file |

## Running locally

Browsers block `fetch()` against local files opened with `file://`, so serve the folder instead of double-clicking `index.html`:

```bash
# Python
python -m http.server 8000

# Node
npx serve .
```

Then open `http://localhost:8000`.

## Deploying to GitHub Pages

1. Push this folder to a GitHub repository (`index.html` and the JSON file at the repo root, or both inside `/docs`).
2. In the repo, go to **Settings → Pages**.
3. Under **Build and deployment**, set **Source** to the branch and folder containing `index.html`.
4. Save. GitHub publishes the site at `https://<username>.github.io/<repo>/`.

No build step is required — the app is static HTML/CSS/JS.

## Question data format

`index.html` fetches `ccaf_questions_consolidated.json` and reads it as either a top-level array or a `{ "questions": [...] }` object. The question count shown on the landing page and throughout the quiz is always computed from this file — it is never hardcoded.

Each question is normalized against several common field-name variants, so the following all work:

| Concept | Accepted keys |
|---|---|
| Question text | `question`, `questionText`, `text`, `prompt`, `title` |
| Options | `options`, `choices`, `answers`, `answerOptions` (array of strings, or objects with `text`/`label`/`value`) |
| Correct answer | `correctAnswer`, `correct_answer`, `correctIndex`, `answerIndex`, `correct`, `answer` — accepted as a letter (`A`–`D`), a numeric index, or the full text of the correct option |
| Explanation | `explanation`, `rationale`, `explain`, `reason`, `description` |

Questions missing required fields (text or at least two valid options) are skipped rather than crashing the app. If the file is missing, unreachable, or contains no valid questions, the app shows an error screen with a **Try Again** button instead of failing silently.

## Quiz behaviour

- One question at a time, with Previous/Next navigation.
- Submitting locks the question: the correct option turns green, an incorrect selection turns red, and the explanation appears.
- Progress bar and "Question X / Total" reflect the live question count from the data file.
- Once every question has been answered, a **View Results** button appears at any point in the quiz.
- The results screen shows the score, percentage, and a clickable list of every missed question — selecting one reopens it in read-only review mode with a **Back to Results** link.
- **Restart Quiz** clears all answers and returns to question 1.
- No timer.
