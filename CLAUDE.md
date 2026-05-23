# 8gents Prototype — CLAUDE.md

## Scope

**All 8gents work must be scoped to this repo** (`skyleong2026-collab/8gents-prototype`), not spark-app.
On claude.ai/code → New session → Repository: `skyleong2026-collab/8gents-prototype`

## The Prototype

Single-file HTML game at `game.html` — no build step, no server required.

### Test locally
```bash
open ~/Documents/8gents-prototype/game.html
# Or via HTTP (for iOS/network access):
cd ~/Documents/8gents-prototype && python3 -m http.server 9000
# → http://192.168.88.6:9000/game.html
```

### Push changes
```bash
git add game.html
git commit -m "your message"
git push
```

GitHub Pages deploys automatically from `main` branch root.
**Live URL**: https://skyleong2026-collab.github.io/8gents-prototype/game.html

## File Structure

```
8gents-prototype/
├── game.html      ← ENTIRE PROTOTYPE — all logic, styles, HTML in one file
├── PROTOTYPE.md   ← Architecture reference (partially outdated; game.html is source of truth)
├── src/           ← Old React Native / Expo attempt — ignore
└── CLAUDE.md      ← This file
```

## §20 Step 1 — Pending Port

The combat event digest (§20 Step 1) was prototyped in spark-app and needs porting into `game.html`:
- **Repo**: spark-app, branch `claude/combat-event-digest-RaquR`
- **Files**: `src/components/BattleVisualization.jsx` + `src/styles/BattleVisualization.css`

Do NOT continue building in spark-app. Port the event digest logic into `game.html`.

## Notion References

- **Session Quick Reference**: https://www.notion.so/36869c57a02d8170b309edba649e2d42
- **Game Design Doc**: https://www.notion.so/36769c57a02d811d9c21ccf850c5923a
- **Test Results & Tuning Log**: https://www.notion.so/36869c57a02d81619d14e52cf1755bc9
- **Bugs & Fixes**: https://www.notion.so/36869c57a02d81eeaa37c37777f87858
