# Prompt Crafter

> You receive scattered thoughts. You ship a battle-ready prompt.

**Prompt Crafter** is an AI skill that transforms fragmented, half-formed ideas into well-structured prompts that any AI agent can execute reliably. Drop in bullet points, rough notes, or vague wishes — it infers your intent, fills in the gaps, and hands back a polished prompt ready for Claude Code, Codex, Cursor, Windsurf, Cline, OpenClaw, Hermes, or any general-purpose LLM.

> **Disclaimer:** This project is developed entirely in my personal time, using my own equipment and resources. It is not affiliated with, endorsed by, or related to my employer. No proprietary company information, code, or resources were used in the creation of this project.

> **Fork note:** koh2035tp adds opt-in Deep Mode (`pc deep` / `pc 深度`) to the original skill by wkloh76. The license and the author credit are unchanged. The install commands below still point at `wkloh76/prompt-crafter`, which does not include Deep Mode. This fork is `https://github.com/koh2035tp/prompt-crafter`.

---

## The Problem

Most people know what they want an AI to do but can't articulate it. They write "make it good" when they mean "validate every input field and return a 400 with a machine-readable error code". They say "analyze the data" when they mean "for each row, flag columns where the 7-day moving average deviates by more than 2 standard deviations".

The gap between human intent and AI-executable instruction is where prompts die. Prompt Crafter closes that gap.

---

## Quick Example

**You say:**
> i need something for my team's code review. we use github, PRs are messy, people don't leave good comments. want a checklist or something that reviewers can follow. maybe also for the author before they submit. nothing too long.

**Prompt Crafter produces:**
```markdown
## Role
You are a code review quality specialist helping a software engineering team
improve their pull request process.

## Task
Create a concise code review checklist for both PR authors and reviewers.
The checklist should fit on one printed page and be easy to follow during
real review sessions.

## Context
- The team uses GitHub for pull requests.
- Current problem: reviews are inconsistent, comments lack substance,
  PR descriptions are often incomplete.
- Used by both authors (before submitting) and reviewers (during review).

## Steps
1. Design two sections: "Before You Submit" and "During Review".
2. For each section, list 5-8 concrete yes/no checklist items.
3. Under each item, add one line explaining why it matters.
4. Add a "Review Quality Guidelines" section at the end.

## Constraints
- Keep the entire document under 500 words.
- No abstract advice — every item must be checkable.
- Use plain language. No unexplained jargon.

## Output Format
A markdown document: # PR Checklist → ## Before You Submit →
## During Review → ## Review Quality Guidelines
```

See [`examples.md`](examples.md) for 10 bilingual (EN/ZH) before/after demonstrations, including one Deep Mode case.

---

## How It Works

Prompt Crafter follows a 4-phase workflow:

| Phase | What Happens |
|---|---|
| **1. Intake** | Absorb everything the user throws at you — no judgment, no interruption. Collect fragments, keywords, constraints, and "not like X" mentions. |
| **2. Triage** | Sort fragments into Goal, Context, Constraints, Input, and Output Shape. Identify gaps with at most 2-3 clarifying questions. |
| **3. Craft** | Build the prompt from a universal template: Role → Task → Context → Steps → Constraints → Output Format → Examples. |
| **4. Deliver** | Present the prompt in a copyable code block, add a one-line summary, and offer iteration. |

Every output is verified against a 9-item quality checklist (clarity, enforceability, ambiguity, contradictions, platform fit, etc.). Deep Mode adds four checks: the five-question pre-analysis was shown, additions are labeled, the prompt names sources and external evidence, and the prompt got deeper rather than longer.

---

## Supported Platforms

Prompt Crafter is a skill definition at its core. Since AI coding platforms don't share a universal instruction format, the project provides self-contained adaptations for each:

| File | Platform | Type |
|---|---|---|
| [`SKILL.md`](SKILL.md) | Frameworks that support YAML-frontmatter skills | Canonical source |
| [`CLAUDE.md`](CLAUDE.md) | [Claude Code](https://claude.ai) | Project instructions |
| [`AGENTS.md`](AGENTS.md) | [Codex (OpenAI)](https://github.com/openai/codex) | Project instructions |
| [`.cursorrules`](.cursorrules) | [Cursor](https://cursor.sh) | Project rules |
| [`.windsurfrules`](.windsurfrules) | [Windsurf](https://windsurf.com) | Project rules |
| [`.clinerules`](.clinerules) | [Cline](https://github.com/cline/cline) | Project rules |
| — | [OpenClaw](https://openclaw.ai), Hermes, ChatGPT, etc. | Use `SKILL.md` as system prompt or reference |

Each file is a standalone, drop-in instruction set. Copy the relevant file into your project root and the platform's AI will follow the prompt-crafting workflow automatically.

---

## Project Structure

```
prompt-crafter/
├── SKILL.md          # Canonical skill definition (YAML-frontmatter)
├── AGENTS.md         # Project documentation + Codex instructions
├── CLAUDE.md         # Claude Code instructions
├── .clinerules       # Cline instructions
├── .cursorrules      # Cursor instructions
├── .windsurfrules    # Windsurf instructions
├── drivers.md       # Collection of trigger phrases (EN/ZH) for testing
├── examples.md      # 10 full before/after examples (bilingual EN/ZH), including Deep Mode
├── CHANGELOG.md      # Version history (Keep a Changelog)
├── README.md         # This file
└── License           # License
```

---

## Installation

### Install with the `skills` CLI (recommended)

The [skills](https://github.com/vercel-labs/skills) CLI (Node.js — run it with `npx`) installs the skill into the directory each agent actually scans.

#### Kimi Code CLI

```bash
npx skills add https://github.com/wkloh76/prompt-crafter -g -a kimi-code-cli -y
```

That is the whole install: one copy at `~/.agents/skills/prompt-crafter`, the user-level skills directory Kimi Code CLI reads. Drop `-g` to install into the current project instead — the skill goes to `.agents/skills/prompt-crafter`, which is committed with the repo and shared with your team.

#### Claude Code, Codex, Cursor

```bash
npx skills add https://github.com/wkloh76/prompt-crafter -g -a claude-code -a codex -a cursor -y
```

Add or drop `-a` flags per client; each one behaves the same way as above. Claude Code keeps its own directory, while Codex and Cursor share `.agents/skills/` — the very copy the Kimi Code CLI install writes — so installing several agents side by side still produces just one copy of the skill plus a symlink for Claude Code.

#### Where the files land (`skills` v1.7.0)

| Agent | `--agent` | Project path | Global path |
|---|---|---|---|
| Kimi Code CLI | `kimi-code-cli` | `.agents/skills/prompt-crafter` | `~/.agents/skills/prompt-crafter` |
| Claude Code | `claude-code` | `.claude/skills/prompt-crafter` (symlink) | `~/.claude/skills/prompt-crafter` (symlink) |
| Codex | `codex` | `.agents/skills/prompt-crafter` | `~/.agents/skills/prompt-crafter` |
| Cursor | `cursor` | `.agents/skills/prompt-crafter` | `~/.agents/skills/prompt-crafter` |

A project install writes `skills-lock.json` at the repo root; a global install writes `~/.agents/.skill-lock.json`. Any other agent: `--agent '*'` (or `--all`) covers every agent the CLI detects. Use `--skill prompt-crafter` to select this skill by its install name (the `name:` field in the frontmatter), `npx skills list` to see what is installed, `npx skills update` to refresh it, and `npx skills remove prompt-crafter` to uninstall.

### Manual install

Clone this repository into your skills directory:

```bash
git clone https://github.com/wkloh76/prompt-crafter.git ~/.agents/skills/prompt-crafter
```

Or download and extract manually:

```bash
mkdir -p ~/.agents/skills/prompt-crafter
cp SKILL.md ~/.agents/skills/prompt-crafter/
```

Then activate the skill in your AI coding tool:

```
/prompt-crafter
```

### Per-project platform files

If your AI coding tool doesn't support native skills, copy the corresponding instruction file into your project root:

```bash
# Claude Code
cp CLAUDE.md /path/to/your-project/CLAUDE.md

# Cursor
cp .cursorrules /path/to/your-project/.cursorrules

# Windsurf
cp .windsurfrules /path/to/your-project/.windsurfrules

# Cline
cp .clinerules /path/to/your-project/.clinerules

# Codex
cp AGENTS.md /path/to/your-project/AGENTS.md
```

The AI will read the file automatically — no activation needed. It will follow the prompt-crafting workflow whenever you ask it to write or refine a prompt.

---

## Usage

Once installed, the skill stays **silent**. It prints nothing on load and never joins a conversation you did not hand it, so your other skills and ordinary requests are untouched. To hand it a request, begin a message with the wake word **`pc`** — e.g. `pc 帮我把这堆零散笔记整理成 prompt` — or send `/prompt-crafter`; everything after the wake word is the request. **`pc help`** (aliases: `pc usage`, `/prompt-crafter help`, `pc 帮助`, `prompt-crafter 用法`) prints the usage guide, and **`pc on`** / **`pc off`** switch ambient mode (vague input auto-activates) on and off for the session.

Examples:

```
/prompt-crafter
pc i need a prompt for my team's code review checklist

pc 帮我写个 prompt，让它根据用户调研提炼核心痛点

pc write a claude code prompt for generating commit messages

pc help        # re-display the usage guide

pc deep 评估在新山办 AI 讲座这门生意能不能做

pc 深度 is a weekend AI seminar in Johor Bahru viable? Deeper, not longer.
```

### Deep Mode · 深度模式

Deep Mode is opt-in. A normal `pc` request does not enter it, and Deep Mode does not change a normal `pc` request.

Start the message with `pc deep` or `pc 深度` when you want a deeper prompt, not a longer one. Intake and Triage still run first. If the Prompt Review Gate applies, that gate still waits for you. Then, before the prompt is written, the skill prints an **Expert Pre-Analysis · 专家预分析**. It answers five questions, at most five bullets each, ranked by importance:

1. What would a top expert in this domain do that you did not ask for? · 这个领域的顶尖专家会做哪些我没要求的事？
2. Which primary sources would they open, instead of material you already digested? · 他们会从哪些最原始的素材入手，而不是我已经整理好的资料？
3. Which concrete methods and tools would they use? · 他们会用哪些具体的方法和工具？
4. What, besides you and the model, can show the result is correct? · 除了你我之外，还有什么可以证明结果是正确的？
5. Which key data are you missing, and how do you get them? · 哪些关键数据我目前没有，该怎么取得？

The rewritten prompt then names those primary sources, methods, and external checks in Context, Steps, and Verification, and the Output Format requires a clear conclusion. This is the one place the skill may add a requirement you did not ask for. Every addition is shown in the pre-analysis and labeled `addition` / `新增` inside the prompt, so you can delete that line. Say which labeled lines to drop and it recrafts without them.

`pc deep` or `pc 深度` with nothing after it asks for the domain (领域) and the original question (我的题目), and does not craft yet.

See [`examples.md`](examples.md) 范例 10 for a full before/after (an AI seminar business in Johor Bahru), and [`drivers.md`](drivers.md) for Deep Mode trigger lines.

See [`drivers.md`](drivers.md) for a full collection of trigger phrases to test with.

## Testing

Use [`drivers.md`](drivers.md) to verify the skill is working. Prefix any line with the wake word (`pc <line>`), paste it into your AI coding tool, and confirm it follows the 4-phase workflow and produces a structured prompt. A bare line without the wake word must produce no reaction at all — that is the negative test. Lines in the Deep Mode section already start with `pc deep` or `pc 深度`; do not put another `pc` in front of them. A normal `pc` in front of the same request must not print the Expert Pre-Analysis.

---

## Design Principles

- **Concrete over abstract.** "Make it good" → specific, measurable criteria.
- **Front-load the task.** The AI should know what to do within the first paragraph.
- **Constraints are gates.** Imperative language: "Do X", "Never Y", "Always Z".
- **Examples over explanations.** One concrete example is worth a thousand words of description.
- **No scope creep.** Refine structure — don't rewrite intent or add unrequested features. Deep Mode is the labeled exception: an expert addition must be shown in the pre-analysis and marked `addition` / `新增`, so you can drop it.
- **Deliberate minimalism.** This is a single-skill project. No build tools, no dependencies, no runtime.

---

## Contributing

Improvements to the skill workflow, new platform adaptations, or additional examples are welcome. Keep changes scoped:

- **To improve the skill:** Edit [`SKILL.md`](SKILL.md) first, then propagate changes to platform files.
- **To add a platform:** Create the appropriate instruction file (e.g., `.aiderules` for Aide) using the existing files as templates.
- **To add examples:** Append to [`examples.md`](examples.md) following the existing format.
- **To add trigger phrases:** Append to [`drivers.md`](drivers.md).
- **To record a change:** Add an entry to [`CHANGELOG.md`](CHANGELOG.md) following [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

No build step required. Changes are effective immediately — the files are consumed as-is by the target platforms.

---

## Changelog

This project follows [Semantic Versioning](https://semver.org/spec/v2.0.0.html). See [`CHANGELOG.md`](CHANGELOG.md) for the full version history.

---

## License

See [`License`](License).
