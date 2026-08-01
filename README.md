<div align="center">
  <img src="https://raw.githubusercontent.com/DNT-Khoa/leetprep/main/media/icon.png" alt="LeetPrep" width="128"/>
  <h1>LeetPrep</h1>
  <p>Scaffold, test, and submit LeetCode problems in Python — with an Anki-inspired rating step that drives a hardest-first practice queue.</p>
</div>

> [!WARNING]
> **🚧 Under active development.** LeetPrep is pre-1.0. Commands, data-file schemas (`.leetprep/reviews.json`, per-problem `.leetprep.json`), and the rating workflow may change between versions without a migration path. Feedback and issue reports are welcome.

## ⚡ What it does

LeetPrep is a VS Code extension for interview prep on LeetCode. It:

- **Scaffolds problems locally** — pick from the LeetCode catalog (or paste a URL) and it creates `<slug>/solution.py` with the official Python starter (plus a Time/Space complexity header) alongside `<slug>/notes.md` with the problem description.
- **Runs & submits from the editor** — ▶ runs your solution against LeetCode's own example test cases via the "Run Code" endpoint; ☁️ submits it for real.
- **Grades your recall** — after every Accepted submission, a modal asks you to rate the attempt (**Hard / Medium / Easy**). Ratings drive the practice panel: 🔴 Hard problems appear at the top, 🟢 Easy at the bottom. Within a group the oldest attempts come first so you don't see the same problem every session.
- **Re-solves on demand** — click any problem in the panel and it silently resets `solution.py` back to the starter so you can re-solve it from scratch. The 🎲 button picks one at random (interview-mode: no idea what's coming).

## 🚀 Install

**Requirements**

- **Python 3.10+** on your PATH (`python3 --version` must work).
- `pip install requests beautifulsoup4` — used by the bundled scripts.
- **VS Code 1.85+**.

Install from the [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=khoa-doan.leetprep) or from the command line:

```bash
code --install-extension khoa-doan.leetprep
```

Then in a new folder run **`LeetPrep: Initialize Workspace`** from the Command Palette to seed `.leetprep/reviews.json`.

## 🔑 LeetCode setup

Test/Submit hit LeetCode's authenticated endpoints, so LeetPrep needs your browser session cookies:

1. Log in to leetcode.com in your browser.
2. Open DevTools → Application → Cookies → `https://leetcode.com`.
3. Copy the values of `LEETCODE_SESSION` and `csrftoken`.
4. Run **`LeetPrep: Set LeetCode Cookies`** from the Command Palette and paste them (they're stored in VS Code's SecretStorage, backed by your OS keychain).

Cookies typically last ~2 weeks. When they expire you'll see an `[auth]` message telling you which one to refresh.

## 🎯 Daily workflow

1. **Add a problem** — `LeetPrep: Search LeetCode` (or `New Problem from URL`). Picks land at `<workspace>/<slug>/` and the `solution.py` auto-opens.
2. **Iterate** — click ▶ to run example cases. Fast, doesn't count as a submission, uses LeetCode's own judge.
3. **Submit** — click ☁️, confirm `y` in the terminal. On Accepted, a modal pops up: **How hard was this?** Pick Hard / Medium / Easy (dismissing defaults to Easy).
4. **Practice later** — open the **LeetPrep** activity-bar entry. Your solved problems are grouped by rating with hardest at the top. Click any row to re-solve (silent reset to starter). Or click 🎲 for a random pick.
5. **Reset manually** — the ↻ button in the editor toolbar resets `solution.py` back to the starter. Panel-click and random-pick do the same silently as part of opening the problem.

## 📋 Commands

| Command | What it does |
|---|---|
| `LeetPrep: Initialize Workspace` | Create `.leetprep/reviews.json` in the current folder |
| `LeetPrep: Search LeetCode` | Fuzzy-search the catalog and scaffold the pick |
| `LeetPrep: New Problem from URL` | Scaffold a specific LeetCode URL |
| `LeetPrep: Run Tests` (▶) | Run example cases via `interpret_solution` |
| `LeetPrep: Submit` (☁️) | Real submission; opens Hard/Medium/Easy modal on Accepted |
| `LeetPrep: Reset Problem` (↻) | Reset `solution.py` to the starter (with confirm) |
| `LeetPrep: Pick Random Practice Problem` (🎲) | Open a random tracked problem for re-solving |
| `LeetPrep: Set LeetCode Cookies` | Paste `LEETCODE_SESSION` + `csrftoken` |

## 🔍 How it works

- Every problem you scaffold gets a hidden `<slug>/.leetprep.json` cache containing the LeetCode `questionId`, `exampleTestcases`, and the original `initialCode`. This means Run / Submit / Reset don't need to re-hit LeetCode's rate-limited GraphQL endpoint on every invocation.
- Ratings and attempt history live in a single `<workspace>/.leetprep/reviews.json` — human-readable, git-committable, portable between machines. This is populated organically as you submit; there is no bulk history import.
- Communication between the Submit script and the VS Code extension happens via `<workspace>/.leetprep/pending.json`: the Python script writes it on Accepted, the extension tails it with a `FileSystemWatcher`, pops the rating modal, and deletes the file.

## 🪪 License

MIT
