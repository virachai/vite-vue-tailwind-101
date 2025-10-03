# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Learning project for building modern UIs with **Tailwind CSS**, **Vue 3**, and **Vite**. This is a minimal starter template demonstrating utility-first CSS with component-based architecture.

## Development Commands

```bash
# Start development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview
```

## Tech Stack

- **Vite** (using Rolldown via `rolldown-vite@7.1.14`)
- **Vue 3** (Composition API with `<script setup>`)
- **Tailwind CSS v4** (via `@tailwindcss/vite` plugin)
- **pnpm** as package manager (see overrides in package.json)

## Architecture

### Entry Point
- [main.js](src/main.js) — Creates Vue app and mounts to `#app`
- [index.html](index.html) — HTML template with Vite script tag
- [style.css](src/style.css) — Global styles (Tailwind CSS should be imported here when configured)

### Component Structure
- [App.vue](src/App.vue) — Root component using `<script setup>` syntax
- [components/](src/components/) — Reusable Vue components

### Tailwind CSS Setup
This project uses **Tailwind CSS v4** via the Vite plugin (`@tailwindcss/vite`). No separate config file is needed—Tailwind v4 uses CSS-based configuration. Import Tailwind directives in `style.css`:

```css
@import "tailwindcss";
```

## Key Notes

- Uses **Rolldown** (Rust-based bundler) instead of standard Vite via pnpm override
- Tailwind v4 requires `@tailwindcss/oxide` to be built (specified in `onlyBuiltDependencies`)
- Vue components use modern Composition API with `<script setup>` syntax
- No custom Tailwind config file—use v4's CSS-based configuration
