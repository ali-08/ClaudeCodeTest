# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A collection of self-contained retro browser games, each built as a single HTML file with inline CSS and JavaScript. No build tools, no dependencies, no frameworks.

## Architecture

Each game is a standalone `<html>` file containing:
- `<style>` block for minimal CSS (fullscreen canvas, hidden cursor)
- `<script>` block with all game logic organized into labeled sections

### Shared Patterns (established in shooter.html, extended in shooter_v2.html)

- **Pixel-art sprites**: 2D arrays using palette keys (e.g., `'G1'`, `'R2'`), rendered via `drawSprite(sprite, x, y, scale, flashWhite)` which maps keys through a `PAL` object to hex colors
- **Audio**: Web Audio API via `playSound(freq, duration, type, vol)` with `ensureAudio()` for user-gesture initialization. SFX are thin wrappers like `sfxShoot()`, `sfxKill()`
- **Input**: Global `keys` object from keydown/keyup, `mouse` object from mousemove, `mouseDown` flag from mousedown/mouseup
- **Game loop**: `requestAnimationFrame` with delta-time capping at 0.05s
- **State machine**: String-based `gameState` variable driving update/render branching
- **Particles**: Array of `{x, y, vx, vy, life, color, size}` objects, spawned via `spawnParticles()`
- **Collision**: Circle-based via `collides(a, b)` comparing squared distances against squared radii sum
- **HUD**: Hearts drawn with `drawHeart()` bezier curves, score/wave text on semi-transparent top bar
- **Screen shake**: `shakeTimer`/`shakeIntensity` applied as random `ctx.translate()` offsets

### shooter_v2.html Structure (sections in order)

1. Constants & Palette → 2. Sprites → 3. Audio → 4. Input → 5. Game State → 6. Sprite Renderer → 7. Level Definitions → 8. Game Flow → 9. Spawning → 10. Shooting → 11. Pickups → 12. Particles → 13. Collision → 14. Update → 15. Boss AI → 16. Rendering → 17. HUD → 18. Screen Renders → 19. Game Loop

## Development

No build step. To test a game:
```
open <filename>.html
```

Games use `localStorage` for high score persistence (keys: `shooterHigh`, `shooterV2High`).

## Git Workflow

- Repository: https://github.com/ali-08/ClaudeCodeTest
- Branch: `main`
- **IMPORTANT**: After every meaningful change (new feature, bug fix, refactor, config update), commit and push to GitHub immediately. Do not let work accumulate uncommitted. Use clean, descriptive commit messages that explain *why* the change was made. This ensures we never lose progress and can easily revert if needed.
