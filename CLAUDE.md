# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Single-file browser game (`index.html`) — no build step, no dependencies, no package manager.

## Development

Open the game directly in a browser:
```
cmd.exe /c start "" "C:\Users\taejl\projects\ClaudeCodeTest\index.html"
```

## Git workflow

After every meaningful change: commit with a clean message and push to `origin/master`.

```bash
git add index.html
git commit -m "short description of change"
git push
```

Remote: https://github.com/taejlee-arch/ClaudeCodeTest

## Architecture

Everything lives in `index.html` as a single self-contained file:

- **CSS** — dark theme, CSS Grid board, hover/win highlight effects
- **JS** — plain vanilla JS, no frameworks. State is `board` (9-element array), `current` (X/O), `over` (bool). `WINS` is a static array of winning index combos checked after each move.
- **HTML** — minimal: `#board` (grid container), `#status` (turn/result text), `#reset` button
