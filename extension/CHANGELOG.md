# Changelog

## 0.2.3

- Phantom's mark is a dot: red for confirmed bugs, amber for warnings, in the CodeLens, the gutter, the
  status bar and the side panel.

## 0.2.2

- Without an AI key Phantom reports "engine only" from the start; the status bar follows the backend's AI
  status.

## 0.2.1

- The Analyst also runs when a file is opened and on "Phantom: Check Intent Now".
- A clear warning when an older backend without the Analyst is still running.

## 0.2.0

- **Analyst**: wrong results detected from the code's own documented examples and from the AI's quoted
  reading of the intent, compared in the sandbox and confirmed by replay; callers re-checked after an edit.
  All ten languages.
- "Wrong result" cards in the Phantom panel.

## 0.1.0

- **Specter**: functions executed on boundary and AI-suggested inputs in a locked-down Docker sandbox;
  failures reported only after reproduction in a fresh container.
- Python, JavaScript, TypeScript, Go, Java, C, C++, Rust, Ruby and PHP.
- The Phantom panel, the welcome walkthrough, depth and tier pickers.
