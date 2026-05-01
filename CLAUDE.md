# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this project is

A single self-contained HTML file — `lmcache-nixl-explainer.html` — that presents an interactive deck explaining how LMCache and NIXL address long-context LLM inference cost. No build step, no dependencies, no server. Open the file directly in a browser.

There is no `package.json`, no test suite, no linter. All CSS and JS live inline in the one HTML file.

## Architecture of the deck

Two concerns dominate the file's structure; understanding them is the difference between safe edits and broken layouts.

**1. Fixed design canvas + per-frame scaling.** Every slide is authored at a literal **1600×900 px** "design pixel" canvas. JS scales the canvas (present mode) or each slide independently (scroll mode) to fit the viewport via `transform: scale(...)`. When adding/editing a slide, work in 1600×900 design pixels — do not introduce viewport units or media queries inside slides. The scaling lives in `fitScale()` / `fitScroll()` / `fit()` near the bottom of the file.

**2. Two render modes share one DOM.** Each `<section class="slide" data-slide="N">` is wrapped at runtime in a `.slide-frame`. CSS class on `<html>` switches behavior:
   - `mode-present`: only `.slide.on` is visible; arrow keys / dot navigator / overview (`O`) / fullscreen (`F`) all work; the canvas is scaled as a whole.
   - `mode-scroll`: every slide-frame becomes a 16:9 box stacked vertically, each containing one independently-scaled slide. The bottom navbar hides; the topbar sticks.

   Initial mode resolves from `?mode=present|scroll` → `localStorage` → default `scroll`. Toggle lives in the topbar.

**Slide registry.** `SLIDE_TITLES` (array of `{num, title, desc}`) is the single source of truth for the dot navigator, overview grid, and counter. The array length **must equal** the number of `<section class="slide">` elements, and the `data-slide` attribute on each section must equal its index in `SLIDE_TITLES` (0-based). When adding a slide, update both.

**Interactivity per slide.** Each interactive slide has its own block of JS further down the file (e.g. `01 — Prefill vs Decode timeline`, prefix-matching demo, sequence diagram, NIXL buffer-list, benchmark chart). They share a few helpers at the top of the script: `$`/`$$`, `fmtMs`, `fmtTok`, `escapeHtml`, and the `COEF` object used by the synthetic prefill/decode timing model. Numbers in the illustrative charts are synthesized to match the qualitative shape of the source talk; the storage-offload benchmark reproduces values shown on screen.

## Brand styling

VAST Data palette, declared as CSS custom properties on `:root`:
- `--vast-deep-blue` `#0a1628` (background), `--vast-navy` `#162a4a` / `--vast-navy-2` `#1c3358` (panels)
- `--vast-cyan` `#00d4ff` / `--vast-teal` `#00a3cc` (accents)
- `--vast-white`, `--vast-silver` `#b8c5d4`, `--vast-line` `#234670`
- Status: `--vast-warn` `#ffb547`, `--vast-bad` `#ff6b6b`, `--vast-good` `#5eead4`
- Fonts: Aptos Display for headings (`--fonth`), Aptos Display / system for body (`--font`), SF Mono / Menlo for code (`--fontmono`)

Use these tokens rather than hardcoding hex values.
