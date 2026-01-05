---
description: Essential context for AI agents working on the Soc Ops codebase. A React-based social bingo game with persistent state and grid-based game logic.
---

# Soc Ops — Copilot Instructions

## ✅ Mandatory Development Checklist

Before committing any changes:
- [ ] `npm run lint` passes
- [ ] `npm run test` passes
- [ ] `npm run build` succeeds

## Quick Start

**Soc Ops** is a 5×5 bingo game for mixers. Find people matching questions to get 5-in-a-row. Built with React 19, TypeScript, Tailwind CSS v4, Vitest, and auto-deploys to GitHub Pages.

## Architecture

**Single-hook state management** in [`useBingoGame.ts`](src/hooks/useBingoGame.ts): manages GameState ('start' | 'playing' | 'bingo'), board, winning line, and localStorage persistence.

**Pure game logic** in [`bingoLogic.ts`](src/utils/bingoLogic.ts): `generateBoard()`, `toggleSquare()`, `checkBingo()`, `getWinningSquareIds()`. Fisher-Yates shuffle randomizes questions; center (index 12) is always free space.

**Components**: `App.tsx` → `StartScreen` | `GameScreen` → `BingoBoard` → `BingoSquare`. `BingoModal` shows victory.

**Key patterns**:
- Immutable state updates via spread/map (never direct mutations)
- Questions from [`src/data/questions.ts`](src/data/questions.ts) (24 + FREE_SPACE)
- Free space always pre-marked; regular squares toggle only if not free space
- localStorage with `validateStoredData()` and SSR guard (`typeof window`)
- Type-safe in [`src/types/index.ts`](src/types/index.ts): BingoSquareData, BingoLine, GameState

## Common Tasks

| Task | File(s) |
|------|---------|
| Add/modify questions | [`src/data/questions.ts`](src/data/questions.ts) |
| Fix game logic bugs | [`src/utils/bingoLogic.ts`](src/utils/bingoLogic.ts) + [`bingoLogic.test.ts`](src/utils/bingoLogic.test.ts) |
| Change UI/styling | [`src/components/`](src/components/) + Tailwind classes in [`src/index.css`](src/index.css) |
| Persist new game data | Update `StoredGameData` type & validation in [`useBingoGame.ts`](src/hooks/useBingoGame.ts) |
| Add game feature | Extend `GameState` type, add logic to `useBingoGame.ts` and `bingoLogic.ts`, then UI component |

## Special Notes

**Tailwind v4**: CSS-first approach via `@theme` directive in [`src/index.css`](src/index.css)—no config file. v4 auto-detects content.

**GitHub Pages**: Auto-deploy on push to `main`. Base path set by `VITE_REPO_NAME` in [`vite.config.ts`](vite.config.ts).

**Testing**: Tests are snapshot-free, behavior-focused. Run `npm run test` to validate game logic.
