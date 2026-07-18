# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

This is **awesome-claude-skills**, a curated "awesome list" of Claude Skills (`README.md`) combined with a large collection of actual, usable skill packages that live directly in this repo. It is not an application — there is no build, lint, or test tooling. The two things you will be asked to do here are almost always: (1) edit `README.md` to add/update a linked skill entry, or (2) create/edit a skill folder (`SKILL.md` + supporting files).

## Repository structure

- `README.md` — the master curated list. Organized into a `## Skills` section with `###` subcategories (Document Processing, Development & Code Tools, Data & Analysis, Business & Marketing, Communication & Writing, Creative & Media, Productivity & Organization, Collaboration & Project Management, Security & Systems, Assistive Technology, App Automation via Composio), followed by `## Getting Started`, `## Creating Skills`, `## Contributing`, `## Resources`, `## License`.
- `CONTRIBUTING.md` — the authoritative rules for what a skill submission must look like and how README entries are formatted (see below).
- Top-level skill folders (e.g. `canvas-design/`, `webapp-testing/`, `theme-factory/`, `brand-guidelines/`, `mcp-builder/`, `slack-gif-creator/`) — first-party skills hosted directly in this repo, each linked from README with a relative path like `./skill-name/`.
- `document-skills/` — a directory grouping the Anthropic document skills (`docx`, `xlsx`, `pptx`, `pdf`), each a full skill folder with its own `SKILL.md`, scripts, and `LICENSE.txt`.
- `composio-skills/` — ~830 auto-generated "App Automation via Composio" skills, one folder per SaaS app (e.g. `gmail-automation/`, `slack-automation/`, `jira-automation/`). These all follow one strict template (see below) and are wired to Composio's Rube MCP server.
- `connect/`, `connect-apps/`, `connect-apps-plugin/` — the "Connect Claude to any app" quickstart skill/plugin referenced at the top of README; `connect-apps-plugin/.claude-plugin/plugin.json` defines it as an installable Claude Code plugin with a `/connect-apps:setup` command.
- `template-skill/` — the minimal skeleton (`SKILL.md` with placeholder frontmatter) to copy when starting a brand-new skill.
- `.github/workflows/label-ready-skill.yml` — CI that validates PRs adding a skill to README (see "README PR validation" below).

Most other top-level folders (`raffle-winner-picker/`, `file-organizer/`, `invoice-organizer/`, `image-enhancer/`, `internal-comms/`, `lead-research-assistant/`, `meeting-insights-analyzer/`, `tailored-resume-generator/`, `twitter-algorithm-optimizer/`, `video-downloader/`, `domain-name-brainstormer/`, `competitive-ads-extractor/`, `content-research-writer/`, `developer-growth-analysis/`, `langsmith-fetch/`, `skill-creator/`, `skill-share/`) are single first-party skills — each is just a `SKILL.md` (occasionally plus scripts/assets/LICENSE.txt).

## Skill folder anatomy

Every skill is a folder containing a `SKILL.md` with YAML frontmatter:

```
skill-name/
├── SKILL.md          # required: name, description frontmatter + instructions body
├── scripts/           # optional: helper scripts
├── templates/          # optional: document templates
└── resources/          # optional: reference files
```

`SKILL.md` frontmatter is minimal — just `name` and `description`. The `description` is what the agent uses to decide when to load the skill (progressive disclosure: only name+description are loaded at session start, ~100 tokens; the full body loads only when relevant; files in `scripts/`/`references/` load on demand). Write descriptions that state clearly *what the skill does and when to use it* — this field is functionally load-bearing, not just documentation.

Composio automation skills (`composio-skills/*-automation/SKILL.md`) use an extended frontmatter with `requires: { mcp: [rube] }` and follow a fixed internal structure: Prerequisites → Setup (Rube MCP at `https://rube.app/mcp`) → Tool Discovery (`RUBE_SEARCH_TOOLS`) → Core Workflow Pattern (discover tools → check connection via `RUBE_MANAGE_CONNECTIONS` → execute). When editing or adding one of these, match the existing pattern in a sibling folder rather than inventing a new structure.

## Adding or editing a skill entry in README.md

Follow `CONTRIBUTING.md` exactly:

1. Pick the correct `###` category (or `##` App Automation subsection for Composio skills).
2. Insert the new bullet in **alphabetical order** within its category — CI enforces this for added lines (see below).
3. Bullet format: `- [Skill Name](url or ./local-path/) - One-sentence description.` Attribute with `*By [@handle](...)*` for external/community skills when known. No emojis; match existing punctuation style exactly.
4. First-party skills hosted in this repo link with a relative path (`./skill-name/`); everything else links to an external GitHub repo/URL.

### README PR validation (`.github/workflows/label-ready-skill.yml`)

A PR that touches README.md and is meant to add a skill listing gets auto-labeled `ready-to-merge` only if **all** of the following hold — know these before proposing a README change, since violating any of them will fail CI:

- The PR changes **only** `README.md` (no other files).
- All diff lines fall strictly between the `## Skills` heading and the `## Getting Started` heading — nothing outside that window may change.
- At least one new bullet line matching `- [Name](url)` is added.
- Every added bullet's URL must be `http(s)://` and must **not** point to a `composio.dev` or `anthropic.com` host (external community links only for this automated flow).
- No added line may contain crypto/web3 terms (`crypto`, `blockchain`, `nft`, `defi`, `token(omics)`, `wallet`, `solana`, `ethereum`, `bitcoin`, etc.) — these are blocked outright.
- Each added bullet must sit alphabetically between its immediate neighbors within its `###` category (pre-existing disorder elsewhere is grandfathered, but new entries must not introduce new disorder).

## Conventions

- License: repo is Apache 2.0 overall; individual skill folders may carry their own `LICENSE.txt` (e.g. `canvas-design/LICENSE.txt`, `document-skills/*/LICENSE.txt`) — check before assuming Apache 2.0 applies to a specific skill's content.
- There is no code to build/lint/test. "Testing" a skill means verifying its `SKILL.md` frontmatter is valid and, per `CONTRIBUTING.md`, that the skill actually works when exercised across the target platform(s) (Claude.ai, Claude Code, and/or the API).
- Keep skill instructions written *for Claude* (imperative, workflow-oriented), not end-user marketing copy — the marketing-style description belongs only in the README bullet.
