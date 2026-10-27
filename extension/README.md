# Phantom

**Execution-verified bug detection for VS Code.** Phantom runs the functions you change against adversarial
inputs in an isolated sandbox, and reports a bug only after it has reproduced the failure a second time in a
fresh container. Every report comes with the input that triggers it and a one-click replay.

![Phantom in VS Code: a wrong result and a crash, each with the input that triggers it](https://d15m91ite2ed7m.cloudfront.net/assets/marketplace/editor.png)

Python · JavaScript · TypeScript · Go · Java · C · C++ · Rust · Ruby · PHP

## What Phantom finds

- **Crashes and hangs.** Uncaught exceptions, non-termination and return values a function's own types rule
  out, found by executing each changed function on boundary inputs (0, -1, empty strings and collections,
  permitted nulls). Typed Python functions are also fuzzed with Hypothesis.
- **Memory errors in C and C++.** Out-of-bounds access, use-after-free and undefined behaviour, caught by
  AddressSanitizer and UndefinedBehaviorSanitizer.
- **Wrong results.** Functions that run fine but return the wrong value: checked against the examples in
  your own comments and docstrings (`>>> f(x)`, `f(x) == y`, `// => y`, `assert_eq!`) and, with AI
  assistance, against the function's stated intent, quoted word for word from your code.
- **Callers broken by an edit.** When a function changes, every function in the file that calls it is
  checked again.

Phantom never claims code is bug-free. An empty report means exactly one thing: no tested input failed.

## How a finding looks

| Mark | Meaning |
|---|---|
| 🔴 Red dot and squiggle | Fails on an input the declared types allow, or contradicts the code's own documented example |
| 🟠 Amber dot and squiggle | Fails only under a stated assumption (untyped code, extreme values), or contradicts the AI's reading of the intent |
| ▶ Replay | Runs the failing input again, in a new sandbox, against the code as it is now |

The **Phantom** panel in the activity bar lists every finding with its input, the observed result and how it
was proven. The status bar shows the counts, the risk of your last edit and the analysis depth.

## Requirements

- **Docker**, running (Docker Engine on Linux; Docker Desktop on macOS and Windows).
- **Python 3.10 or later.**
- An AI key is optional. Without one, Phantom runs its engine alone: boundary inputs, documented examples,
  fuzzing and static hints.

## Getting started

1. Install Docker and Python 3, and make sure Docker is running.
2. Open the Command Palette (**Ctrl+Shift+P**, or **Cmd+Shift+P** on macOS) and run **Phantom: Set Up**. It
   creates Phantom's private Python environment and prepares one locked-down sandbox image per language.
3. Optional: run **Phantom: Set AI Key** to enable AI-suggested inputs and intent checks.
4. Open a file in a supported language. Phantom checks the functions you change each time you pause typing.

## How it stays safe

Your code executes only inside short-lived Docker containers on your own machine, with:

- no network access and a read-only filesystem;
- an unprivileged user (`nobody`), all Linux capabilities dropped and `no-new-privileges`;
- limits on memory, CPU, process count and time per input;
- inputs restricted to plain literals, so no input can carry code of its own.

The local backend listens on `127.0.0.1` only and refuses requests from web pages.

## Layers

| Layer | Runs | Detects | Status |
|---|---|---|---|
| Specter | Every pause in typing | Crashes, hangs, type violations, memory errors | Available |
| Analyst | On open, on save, on request, on risky edits | Wrong results, broken callers | Available |
| Oracle | Highest-risk changes | Timing and concurrency bugs (heisenbugs), with verified fixes | Planned for 0.3 |

Choose the depth and the AI tier for each layer (Swift, Balanced, Deep) from the status bar.

## Commands

| Command | What it does |
|---|---|
| Phantom: Set Up | One-time setup of the Python environment and sandbox images |
| Phantom: Set AI Key | Store the AI key in VS Code's secret storage |
| Phantom: Analyse This File Now | Run Phantom on the current file immediately |
| Phantom: Check Intent Now (Analyst) | Run the Analyst on the current file |
| Phantom: Replay Failing Input | Replay a finding against the current code |
| Phantom: Choose Depth and Tiers | Pick how deep Phantom looks and which tier each layer uses |
| Phantom: Restart Backend | Restart the local analysis backend |
| Phantom: Show Output | Open Phantom's log |
| Phantom: Welcome | Open the getting-started walkthrough |

## Settings

| Setting | Default | Description |
|---|---|---|
| `phantom.enabled` | `true` | Analyse supported files while you type |
| `phantom.depth` | `auto` | How deep Phantom looks |
| `phantom.debounceMs` | `700` | Milliseconds to wait after you stop typing |
| `phantom.useModel` | `true` | Use AI assistance when a key is set |
| `phantom.tiers.specter` / `.analyst` | `swift` / `balanced` | AI tier per layer |
| `phantom.pythonPath` | empty | Python for the backend; empty uses the environment from Set Up |

## Privacy

Execution never leaves your machine. When AI assistance is enabled, the file being analysed (for large files,
the part around the function) is sent to the AI provider to propose inputs and read intent; nothing else from
your project is sent. The AI key is kept in VS Code's secret storage. Website accounts are described in the
[privacy policy](https://d15m91ite2ed7m.cloudfront.net/privacy.html).

## Known limitations

- Instance methods that need a constructed object are skipped; inputs are literals, so code that depends on
  files, sockets or databases is not exercised.
- Caller re-checks cover callers in the same file.
- Docker is required for every language except Python.
- Tested on Linux. macOS and Windows with Docker Desktop are expected to work but have not yet been verified.

## Support

Website: [phantom](https://d15m91ite2ed7m.cloudfront.net) · Contact: [priyanshi.thakor1104@gmail.com](mailto:priyanshi.thakor1104@gmail.com)
