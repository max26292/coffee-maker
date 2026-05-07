# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

A single-file static web app (`index.html`) — a V60 coffee brew guide featuring four championship recipes (Kasuya, Hoffmann, Rao, Winton) with an interactive brew calculator. No build step, no dependencies, no package manager.

Deployed on Vercel as a static site (project: `coffee-maker`).

## Development

Open `index.html` directly in a browser. No server required — all logic is vanilla JS in a `<script>` tag at the bottom of the file.

To preview with Vercel's environment: `npx vercel dev`

## Architecture

Everything lives in `index.html` in three sections:

1. **CSS** (`<style>` block) — design tokens defined as CSS custom properties in `:root` (`--cream`, `--espresso`, `--caramel`, `--gold`). Fonts: Playfair Display (display), DM Mono (monospace data), DM Sans (body).

2. **HTML** — two main sections: `.calculator-section` (brew ratio calculator) and `.recipes-section` (tabbed recipe panels). Each recipe panel has a `.recipe-meta` row, a `.recipe-body` split into step instructions (left) and a sticky `.brew-table-wrap` (right).

3. **JavaScript** (`<script>` block at bottom) — two core functions:
   - `calcAndUpdate()` — reads coffee dose and selected ratio, updates the five calculator result cards, then calls `buildTables()`.
   - `buildTables(coffee, water)` — computes pour amounts for all four recipes and injects HTML into the four `<tbody>` elements (`tbl-kasuya`, `tbl-hoffmann`, `tbl-rao`, `tbl-winton`). Each recipe has its own calculation logic (Kasuya: 40/60 split; Hoffmann: 2× bloom then single pour; Rao: 3× bloom then single pour; Winton: 5 equal pours).
   - `row()` helper — renders a single `<tr>` with a mini bar chart for pour size and cumulative percentage display.

The recipe meta values tagged `.dyn` (coffee/water amounts) are updated dynamically by `buildTables()` alongside the tables whenever the calculator inputs change.
