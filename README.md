# scout

Before you build, find who already solved it. Extract what's worth stealing.

A Claude Code skill for pre-development reference research. Studied 80+ products and repos across 15+ projects. Every Steal List pattern links to the source, names the trade-off, and says what breaks if you copy it blindly.

## What it does

```
/scout
```

Scout asks two questions — what to research (UI/Code/Both) and how deep (Quick/Standard/Deep) — then produces a **Steal List**.

```
STEAL: Command palette with fuzzy search
FROM: Linear (linear.app)
WHAT: Cmd+K opens a searchable command palette that handles navigation, actions, and search in one UI
WHY: Our app has 30+ actions buried in menus. A command palette surfaces them without cluttering the UI
HOW: cmdk library (pacocoursey/cmdk), 2.8KB gzipped, renders as a dialog with filtered list
RISK: Requires consistent action naming. If actions are poorly named, the palette becomes noise
```

Every pattern has a source, a reason, an implementation hint, and a risk. No vague "consider using X."

<details>
<summary>Full output example (Steal List + Kill List + Eureka)</summary>

```
STEAL: fcntl-based work queue for parallel agents
FROM: Dicklesworthstone/claude_code_agent_farm (777 stars)
WHAT: Multiple Claude sessions claim work chunks via POSIX advisory locks on a shared JSON file
WHY: Our team templates spawn parallel agents but have no file collision prevention
HOW: fcntl.flock() on state.json. Each agent: lock → read queue → claim chunk → unlock
RISK: macOS/Linux only. Windows needs msvcrt.locking fallback

STEAL: stagger delay between agent spawns
FROM: Dicklesworthstone/claude_code_agent_farm
WHAT: 10-second delay between spawning agents to avoid API rate limit bursts
WHY: Spawning 5 agents simultaneously hits rate limits
HOW: "stagger_seconds": 10 in template config, spawner sleeps between launches
RISK: None. Pure improvement

KILL: Self-learning WASM kernel architecture
SEEN IN: ruvnet/ruflo (30K stars)
WHY NOT: Enterprise platform complexity. Our positioning is "simplest team booster" — adding WASM
         and Q-learning routers turns us into a worse version of ruflo

EUREKA: Every competing repo adds more agents and more complexity. 33K-star repo has 182 agents.
        30K-star repo has WASM kernels. But Claude Code already provides orchestration natively.
        What users actually lack is "how do I start a team in 30 seconds" — not more power, less friction.
```

</details>

## Install

Paste this into Claude Code:

```
Read https://raw.githubusercontent.com/Hybirdss/scout/main/scout/SKILL.md and save it to ~/.claude/skills/scout/SKILL.md
```

Or manually:

```bash
git clone https://github.com/Hybirdss/scout.git ~/dev/scout
ln -s ~/dev/scout/scout ~/.claude/skills/scout
```

## How it works

```
/office-hours → /scout → /plan-ceo-review → /plan-eng-review → build → /ship
  idea          references    scope            architecture      code    deploy
```

1. **Phase 0** — Reads your project context (README, package.json, office-hours output)
2. **Phase 1** — Asks what to scout (UI/Code/Both) and how deep (Quick/Standard/Deep)
3. **Phase 1.5** — Auto-detects build type from project files, adapts search strategy
4. **Phase 2** — UI/UX research: WebSearch + gstack browse screenshots + pattern extraction
5. **Phase 3** — Code research: GitHub repos + deep-read implementations + comparison matrix
6. **Phase 4** — Writes Steal List + Kill List + reference document

Output saved to `~/.gstack/projects/{slug}/references/`.

<details>
<summary>Build-type-aware search</summary>

Scout adapts where it looks based on what you're building:

| Building | Where scout looks | What it extracts |
|----------|------------------|-----------------|
| SaaS / Web app | GitHub trending, competitor sites | Auth, pricing, onboarding, data model |
| CLI tool | `awesome-*` lists, similar CLIs | Command structure, flags, output format |
| API / Backend | OpenAPI specs, similar providers | Routes, auth, rate limiting, pagination |
| AI agent / Skill | gstack skills, LangChain, MCP servers | Workflow phases, prompt structure |
| MCP server | modelcontextprotocol org repos | Tool design, auth flow, response schema |
| Library / SDK | npm/PyPI popular packages | API surface, TypeScript types |
| UI component | Radix, shadcn/ui, Ant Design | Component API, composition, accessibility |
| Mobile app | App Store top charts | Navigation, gestures, offline handling |
| Browser extension | Chrome Web Store | Manifest, content scripts, storage |
| Game | itch.io, Love2D/Godot examples | Game loop, input handling, asset pipeline |

</details>

## Depth levels

| Depth | References | Analysis | Eureka |
|-------|-----------|----------|--------|
| Quick | Top 3, skim | 1-layer synthesis | Optional |
| Standard | Top 5, deep-read top 3 | 3-layer synthesis | Welcome |
| Deep | 8-10, full code analysis | 3-layer + comparison matrix | Minimum 2 required |

## Philosophy

Based on gstack's [Search Before Building](https://github.com/garrytan/gstack/blob/main/ETHOS.md) — one of three core builder principles.

Three layers of knowledge:
1. **Layer 1 (tried and true)** — Don't reinvent.
2. **Layer 2 (new and popular)** — Scrutinize.
3. **Layer 3 (first principles)** — Prize above all.

The most valuable outcome is a **Eureka** — a reason the conventional approach is wrong for your product.

## Limitations

- **No private repos.** Scout reads public GitHub repos and websites. Private codebases require manual access.
- **Browse optional.** Visual analysis uses gstack's browse binary (`$B`). Without it, falls back to WebSearch for screenshots.
- **GitHub rate limits.** Heavy use of `gh search` can hit API limits. Quick depth avoids this.
- **English-biased search.** WebSearch and GitHub queries work best in English. Non-English ecosystems may need manual search terms.

## gstack compatibility

- Saves to `~/.gstack/projects/{slug}/references/` — gstack's artifact path
- Uses gstack browse (`$B`) for visual analysis (optional)
- Follows ETHOS.md Three-layer synthesis + Eureka patterns
- Feeds into `/plan-eng-review` and `/plan-design-review`

## Changelog

- **v2.0** — Depth selection (Quick/Standard/Deep), build-type auto-detect, gstack browse only, Deep mode Eureka minimum
- **v1.0** — Initial release. Two modes (UI/Code), Steal List format, 10 build types, Three-layer synthesis

## License

MIT
