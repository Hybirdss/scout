# scout

Pre-development reference research for Claude Code. Produces a **Steal List** — concrete patterns extracted from real products and real codebases, with evidence.

## What it does

Before you build, scout finds who already solved your problem and extracts what's worth stealing.

```
/scout
```

Two modes:
- **UI/UX references** — screenshot and analyze the best products in your space
- **Code references** — find repos on GitHub, read their implementations, compare architectures

Output: a structured reference document with a Steal List at `~/.gstack/projects/{slug}/references/`.

## The Steal List

The primary deliverable. For each pattern worth adopting:

```
STEAL: Command palette with fuzzy search
FROM: Linear (linear.app)
WHAT: Cmd+K opens a searchable command palette that handles navigation, actions, and search in one UI
WHY: Our app has 30+ actions buried in menus. A command palette surfaces them without cluttering the UI
HOW: cmdk library (pacocoursey/cmdk), 2.8KB gzipped, renders as a dialog with filtered list
RISK: Requires consistent action naming. If actions are poorly named, the palette becomes noise
```

## Build-type-aware search

Scout adapts its search strategy based on what you're building:

| Building | Where scout looks | What it extracts |
|----------|------------------|-----------------|
| SaaS / Web app | GitHub trending, competitor sites | Auth, pricing, onboarding, data model |
| CLI tool | `awesome-*` lists, similar CLIs | Command structure, flags, output format |
| API / Backend | OpenAPI specs, similar providers | Routes, auth, rate limiting, pagination |
| AI agent / Skill | gstack skills, LangChain, MCP servers | Workflow phases, prompt structure, tool selection |
| MCP server | modelcontextprotocol org repos | Tool design, auth flow, response schema |
| Library / SDK | npm/PyPI popular packages | API surface, TypeScript types, docs structure |
| UI component | Radix, shadcn/ui, Ant Design | Component API, composition, accessibility |
| Mobile app | App Store top charts | Navigation, gestures, offline handling |

## Philosophy

Based on gstack's [Search Before Building](https://github.com/garrytan/gstack/blob/main/ETHOS.md) principle — one of gstack's three core builder principles.

Three layers of knowledge:
1. **Layer 1 (tried and true)** — Standard patterns everyone uses. Don't reinvent.
2. **Layer 2 (new and popular)** — Ecosystem trends. Scrutinize, don't blindly adopt.
3. **Layer 3 (first principles)** — Original observations about THIS problem. Prize above all.

The most valuable outcome is a **Eureka** — a reason the conventional approach is wrong for your product.

## Pipeline position

scout fills the gap between ideation and planning:

```
/office-hours → /scout → /plan-ceo-review → /plan-eng-review → build → /ship
  idea          references    scope            architecture      code    deploy
```

## Install

```bash
# Copy the skill to your Claude Code skills directory
cp -r scout ~/.claude/skills/scout

# Or symlink for easy updates
ln -s "$(pwd)" ~/.claude/skills/scout
```

## gstack compatibility

- Saves output to `~/.gstack/projects/{slug}/references/` — the same path gstack uses for project artifacts
- References gstack's browse binary for visual analysis (optional — works without it)
- Follows gstack's Three-layer synthesis and Eureka patterns from ETHOS.md
- Designed to feed into `/plan-eng-review` and `/plan-design-review`

## License

MIT
