# Personal Memex

Personal knowledge base built on [Quartz 4](https://github.com/jackyzha0/quartz) — Markdown notes published as a navigable website with graph view, backlinks, and full-text search.

## Prerequisites

- Node.js >= 22 (`v22.16.0` pinned in `.node-version`)
- npm >= 10.9.2

## Setup

```bash
npm install
```

## Add Content

Drop Markdown files into the `content/` folder. Supports Obsidian-flavored Markdown, wikilinks, tags, and frontmatter.

```
content/
  index.md          # home page
  notes/
    my-note.md
```

Minimal frontmatter:

```yaml
---
title: My Note
date: 2026-05-09
tags: [topic, subtopic]
---
```

## Run Locally

```bash
npx quartz build --serve
```

Opens at `http://localhost:8080`. Hot-reloads on file changes.

## Build

```bash
npx quartz build
```

Output goes to `public/`.

## Configure

| File | Purpose |
|------|---------|
| `quartz.config.ts` | Site title, base URL, plugins, theme, analytics |
| `quartz.layout.ts` | Page layout components |

Before deploying, update these fields in `quartz.config.ts`:

```ts
pageTitle: "Your Site Name",
baseUrl: "yourdomain.com",
```

## Obsidian Setup

Point Obsidian at the `content/` folder — not the repo root.

1. Open Obsidian → **Open folder as vault**
2. Select `content/` inside this repo
3. Done — all notes you write in Obsidian land directly in `content/`

Obsidian's `.obsidian/` config folder will be created inside `content/` — it's already in `ignorePatterns` in `quartz.config.ts` so Quartz won't publish it.

### Push to GitHub from Obsidian

Use the [Git](https://github.com/Vinzent03/obsidian-git) plugin by Vinzent.

**Install:**
1. Obsidian → Settings → Community plugins → Browse → search **Git** by Vinzent → Install → Enable

**Configure** (Settings → Obsidian Git):
- Auto pull interval: `10` min (optional)
- Auto push interval: `0` (manual) or set a value for autopush
- Commit message: `vault backup: {{date}}`

**Usage:**
- `Cmd+P` → `Git: Commit all changes` — stages and commits
- `Cmd+P` → `Git: Push` — pushes to GitHub

Or enable **auto backup** in plugin settings to commit+push on a timer without thinking about it.

> The plugin runs git from the vault root (`content/`), but your `.git` folder is one level up (repo root). Set **Custom base path** in plugin settings to `../` so it finds the correct git repo.

## Deploy

Any static host works (Cloudflare Pages, Vercel, Netlify, GitHub Pages).

Build command: `npx quartz build`
Output directory: `public`

For GitHub Pages, see the [Quartz hosting docs](https://quartz.jzhao.xyz/hosting).
