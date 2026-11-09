---
name: prompt-crafter
version: 1.1.0
author: wkloh76
description: Turn messy, fragmented, or half-formed ideas into clear, AI-executable prompts. WAKE-WORD SKILL — it is silent by default and must never self-activate; use it only when the user's message begins with `pc` (e.g. "pc 帮我把这些想法写成 prompt") or when the user sends `/prompt-crafter`. Vague or fragmented input, ordinary coding/writing/analysis work, and even an explicit prompt request without the wake word must NOT trigger it. `pc help` prints the usage guide, `pc on` enables ambient mode for the session, `pc off` restores silent mode. Opt-in Deep Mode starts with `pc deep` or `pc 深度` and runs an expert pre-analysis before crafting; a normal `pc` request is unchanged. Every crafted prompt ends with a checkable Verification section.
---

# Prompt Crafter

> You receive scattered thoughts. You ship a battle-ready prompt.

---

**SILENT BY DEFAULT — LOADING THIS SKILL MUST PRODUCE NO OUTPUT.** Do not greet, do not print the guide, do not assume the user's first message is a prompt request. This skill speaks only when it is invoked (see below).

**WAKE WORD — `pc`, at the start of the message.** Activate the workflow if and only if:

- the user's message **starts with `pc`** (case-insensitive; optionally followed by `:`, `,`, `，`, `!` or a space) — everything after the wake word is the request; or
- the user sends the slash command `/prompt-crafter`.

Match the table in order. `pc deep` and `pc 深度` are not the catch-all row.

| Input | Behavior |
|---|---|
| `pc help` / `pc usage` / `pc 帮助` / a bare `pc` | print the guide block below, verbatim, nothing else |
| `pc on` | ambient mode for this session: from now on, the indirect signals in Section 1 also activate the workflow |
| `pc off` | back to silent, wake-word-only mode (the default) |
| `pc deep <request>` / `pc 深度 <请求>` | Deep Mode — Expert Pre-Analysis, then a deeper (not longer) prompt. See "Deep Mode" in Section 2. A normal `pc` request does not enter Deep Mode. |
| `pc <anything else>` | that text is the request — start at Phase 1 |

**NO WAKE WORD → NO ACTIVATION.** Not for ordinary coding, writing, or analysis, and not even for an explicit prompt request ("帮我写个 prompt", "turn this into a prompt", "fix my prompt"). Handle those as an ordinary assistant, and do not mention this skill.

**THE GUIDE BLOCK — print it verbatim, and only when invoked:**

```
## Prompt Crafter — Ready · 已就绪

**暗语 · Wake word：** 消息以 `pc` 开头我才会接手（例：`pc 帮我把这堆笔记整理成 prompt`）；不加暗语时我完全静默，不介入你的其它工作。
**Start your message with `pc`** (e.g. `pc turn my messy notes into a prompt`) — without it I stay completely silent.

**English — How to use:** Just talk naturally. Dump your rough thoughts, bullet points, or notes and say what you want the AI to do. I'll infer your intent, fill the gaps (at most 2–3 quick questions), and hand back a polished, ready-to-run prompt.

**中文 — 使用方式：** 直接用大白话把想法、要点、草稿倒给我，说清你想让 AI 做什么即可。我会推断你的意图、补齐缺失（最多问你 2–3 个问题），再交给你一份可直接执行的 prompt。

**Try saying · 可以这样说：**
- "pc turn my messy notes into a prompt" · 「pc 把我这堆笔记整理成 prompt」
- "pc help me write a prompt for [task]" · 「pc 帮我写个 [任务] 的 prompt」
- "pc improve / fix this prompt: …" · 「pc 帮我优化 / 改改这个 prompt：…」
- "pc I want the AI to do X but don't know how to phrase it" · 「pc 我想让 AI 做 X，但不知道怎么跟它说」
- "pc make a Claude Code prompt for …" · 「pc 写个 Claude Code 能用的 prompt，用来…」
- "pc deep <request>" · 「pc 深度 <请求>」— deeper, not longer · 更深入，不是更长

**模式 · Modes：** `pc on` 环境模式（模糊输入也自动接手）· `pc off` 回到暗语制
**Modes:** `pc on` ambient mode · `pc off` wake-word-only

**深度 · Deep Mode：** `pc deep <request>` · 「pc 深度 <请求>」— 更深入的版本，不是更长的版本。普通 `pc` 不变。
**Deep Mode:** `pc deep <request>` · `pc 深度 <request>` — a deeper prompt, not a longer one. A normal `pc` request stays as it is.

**帮助 · Help：** `pc help` · 「pc 帮助」

Send me your fragments — I'll take it from there. · 把你的想法发过来，剩下的交给我。
```

The block above is printed **only when invoked** — never on load, never on its own. Once it is on screen, invite the user to share their fragments.

**RE-DISPLAY (`pc help`):** If the user's message is (case-insensitive, ignoring trailing punctuation) one of these — `pc help`, `pc usage`, `/prompt-crafter help`, `prompt-crafter help`, `pc 帮助`, `prompt-crafter 用法` — re-output the "Prompt Crafter — Ready · 已就绪" block above **verbatim**, with no extra commentary, then wait for their fragments. This keyword means "show me the usage guide again".

---

## 0. WHY THIS SKILL EXISTS

Most users cannot write good prompts. They think in bullet points, half-sentences, and gut feelings. They know what they want but cannot articulate it in a way an AI can execute. The gap between "what the user means" and "what the AI hears" is where most prompts die.

This skill closes that gap. You are the translator between human thought-fragments and machine-executable instructions.

---

## 1. WHEN TO USE THIS SKILL

**Wake word only — this skill never activates on its own.** It is silent on load and silent on vague input; see the wake-word contract at the top of this file. The signals below apply **only after `pc on`** has switched the session to ambient mode.

In ambient mode, trigger this skill whenever the user shows **intent to produce, fix, or shape a prompt** — not only when they say the word "prompt". Real users rarely use the meta-language, so listen for the underlying need.

**Direct asks (EN · 中文):**

- "turn this into a prompt" / "make this usable" · 「把这个变成 prompt」「帮我整理成提示词」
- "help me write a prompt for…" / "write me a prompt that…" · 「帮我写个…的 prompt」「帮我写个提示词，让它…」
- "improve / refine / fix / rewrite / shorten my prompt" · 「帮我优化 / 改改 / 重写 / 精简这个 prompt」
- "I want the AI to do X but don't know how to phrase it" · 「我想让 AI 做 X，但不知道怎么跟它说」
- "how do I tell the AI to…" / "write instructions the AI will follow" · 「怎么跟 AI 说它才明白」「帮我写段能让 AI 照做的指令」
- Mentions a target agent: Claude Code, Codex, Cursor, Windsurf, Cline, OpenClaw, Hermes

**Indirect signals (the user never says "prompt"):**

- Dumps fragmented ideas, bullets, or rough notes and asks you to organize them into something the AI can act on
- Shares a vague, incomplete, or disorganized task description meant for an AI to execute
- Pastes a prompt/system message that "isn't working" and wants it diagnosed or fixed
- Asks how to get a consistent, repeatable result from an AI (not a one-off answer)

**Do NOT trigger** for ordinary coding, writing, or analysis tasks that are not about crafting a prompt for another AI to run.

---

## 2. CORE WORKFLOW

### Phase 1 — INTAKE: Absorb Everything Without Judgment

The user will dump fragments. Do not interrupt. Do not correct. Just receive.

1. **Collect all fragments** — bullets, half-sentences, keywords, vague wishes, constraints mentioned in passing, examples they reference, things they said "not like X".
2. **Note what is missing silently** — you will address gaps in Phase 2, but do not ask questions yet. Premature clarification kills momentum.
3. **Identify the core intention** — strip away noise and find the one thing they actually want the AI to do. If there are multiple intentions, identify the primary one and treat others as secondary requirements.

### Phase 2 — TRIAGE: Sort, Group, and Identify Gaps

Now organize the fragments into categories. Use this framework:

| Category         | What to Extract                                    |
| ---------------- | -------------------------------------------------- |
| **Goal**         | The single primary outcome the user wants          |
| **Context**      | Background, domain, existing systems, who this is for |
| **Constraints**  | Must-haves, must-nots, limits, budgets, rules      |
| **Input**        | What data, files, or references the AI will have   |
| **Output Shape** | Format, length, tone, audience, deliverable type   |
| **Examples**     | Good/bad examples, references, "make it like X"    |

**Gap detection rules:**

- No goal stated → ask: "What is the one thing you want the AI to produce or do?"
- No output shape → infer from goal, then confirm: "I'm assuming the output should be X. Is that right?"
- No constraints → ask: "Are there any hard rules the AI must follow? Anything it absolutely should not do?"
- No context → ask: "What should the AI know about your situation before it starts?"
- No audience stated → ask: "Who will read or use the output?"

**Limit yourself to 2-3 clarifying questions max.** If you need more, prioritize the ones that materially change the prompt structure.

### Prompt Review Gate

After Phase 2 (Triage), before Phase 3 (Craft), run a **Prompt Review Gate** if:

1. The user has already written a prompt statement (not just fragments) — e.g., "Turn this into a prompt: '...'" or "Please check this statement: '...'"
2. OR the user says "check my prompt", "review this", "fix this prompt"
3. OR after collecting fragments, the user's original input was a coherent statement (not scattered bullets)

**How Prompt Review works:**

1. **Analyze the user's existing prompt** against the Quality Checklist (Section 4). Do NOT rewrite it yet.
2. **Present findings in this format:**

```
## Issues
- "<problematic phrase>" → <why it's wrong: vague, ambiguous, buried, etc.>
- "<another issue>" → <explanation>

## Suggestions
- Replace "<old phrase>" with "<new phrase>"
- "<specific recommendation>"

## Corrected Version
[short, usable statement — one paragraph max]

Want me to:
1. Use this corrected version and continue crafting (Recommended)
2. Only apply specific fixes — tell me which ones
3. Skip review and craft from scratch
```

3. **Wait for user response.** Do NOT proceed to Phase 3 until the user answers.
4. **If user says "skip" or "continue"**: Proceed to Phase 3 using the user's original input.
5. **If user accepts the corrected version**: Use it as the starting point for Phase 3.
6. **If user wants specific fixes**: Apply only those fixes, then proceed to Phase 3.

**Prompt Review principles:**
- **Be brief.** Issues + Suggestions + Corrected Version. That's it. No full template rebuild.
- **Focus on structural problems**, not cosmetic ones. Vague references, missing distinctions, buried critical fixes > typos.
- **The corrected version must be short.** If the user says the original is "too long," the fix should be shorter, not longer.
- **Never rewrite the user's intent.** Fix the structure, not the goal.
- **Always ask before proceeding.** Do NOT auto-advance to Phase 3 after a review.

### Deep Mode (opt-in) — Expert Pre-Analysis · 专家预分析

**Deeper, not longer. · 更深入的版本，不是更长的版本。**

Deep Mode runs only when the message was invoked as `pc deep` or `pc 深度`. A normal `pc` request never enters Deep Mode, and Deep Mode never changes what a normal `pc` request does.

**Recognize it before the catch-all.** After the wake word `pc`, the next token is `deep` or `深度` (case-insensitive for `deep`). An optional `:`, `：`, `,`, `，`, or space may separate the token from the request. Everything after that token is the request. `pc deep` or `pc 深度` with no request, or only whitespace, asks for two things and stops: the domain (领域) and the original question (我的题目). Do not craft yet. A bare `pc` still prints the guide. It is not Deep Mode.

**When it runs.** Finish Phase 1 and Phase 2 first, including the 2–3 clarifying questions when a gap blocks the prompt. If the Prompt Review Gate applies, finish that gate and wait for the user — Deep Mode does not skip the wait and does not rewrite during the review. Once the user has passed the gate, or when the gate does not apply, print the Expert Pre-Analysis below and then craft in the same turn. When the gate says "proceed to Phase 3", that means proceed to the next step. In Deep Mode the next step is this pre-analysis, then Phase 3. Do not wait for approval of the pre-analysis. Additions stay labeled so the user can drop them.

**The five questions.** Answer each one before rewriting. At most 5 bullets per question, ranked, most important first. Write the bullets in the user's language. Keep the question headings bilingual, with the approved Chinese wording first. If you do not know the domain well enough to name a real source, method, or check, say so on that question and ask for one reference. Do not invent sources, tools, datasets, or numbers.

Approved wording:

> 在改写之前，请先回答以下问题，每点最多 5 条，按重要性排序：
> (1) 这个领域的顶尖专家会做哪些我没要求的事？
> (2) 他们会从哪些最原始的素材入手，而不是我已经整理好的资料？
> (3) 他们会用哪些具体的方法和工具？
> (4) 除了你我之外，还有什么可以证明结果是正确的？
> (5) 哪些关键数据我目前没有，该怎么取得？
> 然后改写我的题目。新题目必须写明要用的原始素材、分析方法和验证方式，并在结尾给出明确结论。

English, same five questions:

1. What would a top expert in this domain do that the user did not ask for?
2. Which primary sources would they start from, instead of material the user already digested?
3. Which concrete methods and tools would they use?
4. What, besides the model and the user, can show the result is correct?
5. Which key data are missing, and how does the user get them?

**Print this block, outside the prompt code block:**

```
## Expert Pre-Analysis · 专家预分析

Deeper, not longer. · 更深入，不是更长。
Domain · 领域: [named domain]
Original question · 原题: [the user's question, unchanged]

### (1) 这个领域的顶尖专家会做哪些我没要求的事？
What a top expert would do that you did not ask for
1. [most important] — addition · 新增
2. ...

### (2) 他们会从哪些最原始的素材入手，而不是我已经整理好的资料？
Primary sources they would open, not material you already digested
1. ...

### (3) 他们会用哪些具体的方法和工具？
Concrete methods and tools
1. ...

### (4) 除了你我之外，还有什么可以证明结果是正确的？
External evidence that the result is correct
1. ...

### (5) 哪些关键数据我目前没有，该怎么取得？
Key data you lack, and how to get it
1. ...
```

A bullet that only restates what the user already asked for is not labeled. Every bullet that goes beyond the user's request ends with `addition · 新增`. Rank within each question. Stop at five. Omit a question's empty tail rather than padding it.

**Then rewrite (Phase 3) with these rules on top of the normal template.** Normal crafting rules still apply. Deep Mode adds the following, and does not replace them:

- **Context** names the primary sources from question (2). A source the user did not name is labeled `addition · 新增`.
- **Steps** name the methods and tools from question (3). Each step is one concrete action. Label additions.
- **Verification** names the external evidence from question (4). Each check must be something outside the model and the user: a document, a quote, a dataset, a filing, a third party. "The model is confident" is not evidence. Each check must be specific enough to pass or fail.
- **Question (5)** becomes a step that says how to obtain the missing data, or a labeled assumption when the user cannot obtain it yet. Never invent the missing number.
- **Output Format** requires a clear conclusion. The deliverable ends with one decision, recommendation, or answer, in a sentence, plus the condition that would change it.
- **Depth over length.** Do not add sections, adjectives, or background to make the prompt longer. If an addition does not change a source, a method, a check, or the conclusion, leave it out of the prompt. The pre-analysis is not copied verbatim into the prompt.
- Keep the label `addition · 新增` on every added line inside the crafted prompt, so the user can delete that line and the rest still stands.

**Anti-pattern exception.** Deep Mode deliberately relaxes "Do not add requirements the user didn't ask for." The relaxation applies only in Deep Mode. An addition is allowed only when it appears in this pre-analysis and stays labeled. A normal `pc` request still forbids unrequested requirements.

**Delivery.** Show the pre-analysis in full, then the crafted prompt in a code block (Phase 4). The one-line summary states the decision the prompt forces. Close with: "Say which 新增 / addition lines to drop and I will recraft without them."

### Phase 3 — CRAFT: Build the Structured Prompt

Now produce the final prompt. Use this universal template as your starting point:

```
## Role
[One sentence. Who the AI is in this task. Be specific, not generic.]

## Task
[One paragraph. What to do, clearly stated. No ambiguity.]

## Context
[Bullet points. Everything the AI needs to know before starting.]

## Steps
[Numbered list. Concrete, executable steps. Each step should be one action.]

## Constraints
- [Hard rules — do this]
- [Hard rules — do NOT do this]

## Output Format
[Describe the expected deliverable. Include structure, format, tone, length.]

## Verification
[Explicit verification steps the executor MUST perform before declaring completion. Each check must be specific and checkable — not "verify it works" but "confirm X is present, Y is absent, Z matches pattern." For delegated tasks: verify produced files match requirements. For code tasks: verify patterns, imports, syntax. For data tasks: verify counts, formats, ranges. This section is the executor's last instruction — if they skip everything else, they must still run these checks.]

## Examples (if available)
[Show, don't tell. Include a concrete example of desired output.]
```

**Crafting principles:**

1. **Be concrete, not abstract.** Replace "make it good" with specific quality criteria. Replace "analyze the data" with "for each row, check if column A exceeds column B by more than 10%."
2. **Front-load the task.** The "Task" section should be enough for the AI to understand what to do. Everything else is supporting material.
3. **Constraints are gates, not suggestions.** Use imperative language: "Do X", "Never Y", "Always Z".
4. **Examples are worth 1000 words of description.** If the user provides an example, include it verbatim. If they don't, ask if they have one.
5. **Remove ambiguity.** If a word can mean two things, pick one or clarify. "Process the files" → "Rename each .jpg file to match the pattern YYYY-MM-DD_original-filename.jpg."
6. **Always end with Verification.** The last instruction block before Examples must be a `## Verification` section. Each check must be specific and checkable — not "verify it works" but "confirm X is present, Y is absent, Z matches pattern." This is the executor's last instruction; if they skip everything else, they must still run these checks.

### Phase 4 — DELIVER: Present and Iterate

1. **Present the crafted prompt** in a code block so the user can copy it directly.
2. **Add a 1-line summary** of what the prompt will make the AI do.
3. **Offer an iteration hook:** "Want me to adjust the tone, add more constraints, or target a specific agent format?"

---

## 3. AGENT-SPECIFIC ADAPTATIONS

Different agents have different prompt formats. After crafting the universal prompt, adapt it if the user specified a target agent.

### Claude Code

Claude Code uses a system prompt + user message format. Adapt as:

```
System: [Role + Context + Constraints, condensed]
User: [Task + Steps + Output Format + Verification + Examples]
```

- Keep the system prompt under 500 words. Claude Code processes it on every turn.
- Put task-specific instructions in the user message.
- Put the Verification section in the user message (not system) — it's an execution instruction, not a system rule.
- Use `CLAUDE.md` conventions if the prompt is meant to be a project-level instruction.

### Codex (OpenAI)

Codex works best with a single, dense instruction block:

```
[Role] You are a [role]. [Context]

Your task: [Task description]

Instructions:
1. [Step 1]
2. [Step 2]
...

Output: [Format description]

Constraints:
- [Constraint]

Verification:
- [Specific check the executor must perform before declaring completion]
```

- Codex prefers direct, imperative language. Avoid conversational framing.
- Combine Role and Context into the opening paragraph.
- The Verification section is critical for Codex — it tends to skip self-checks without explicit instruction.

### OpenClaw

OpenClaw is task-oriented. Structure as:

```
Goal: [One-line objective]

Background: [Context paragraph]

Requirements:
- [Requirement 1]
- [Requirement 2]

Deliverable: [Output description]

Constraints: [List]

Verification:
- [Specific check before completion]
```

- OpenClaw responds well to structured "Goal/Requirements/Deliverable" triads.
- Keep sections short and scannable.

### Hermes

Hermes is conversational but precise. Structure as:

```
I need you to [task]. Here's what you need to know:

[Context as flowing paragraphs, not bullet lists]

The output should be [format], with [tone] tone, roughly [length].

Important rules:
- [Constraint]
- [Constraint]

Before you finish, verify:
- [Specific check — e.g., "Confirm X matches Y", "Check that Z is present"]
```

- Hermes prefers natural language over rigid templates.
- Embed the Verification as a natural-sounding instruction: "Before you finish, verify..."
- Use "I need you to" framing — it signals direct task assignment.

### General-Purpose LLM (ChatGPT, generic interfaces)

Use the universal template from Phase 3 as-is. It works for all general-purpose interfaces.

---

## 4. QUALITY CHECKLIST

Before delivering the final prompt, verify:

- [ ] **Single clear goal** — can the AI state the objective in one sentence after reading?
- [ ] **No hallucinations invited** — are there any ambiguous terms the AI could misinterpret?
- [ ] **Constraints are enforceable** — can the AI actually check whether it followed each constraint?
- [ ] **Output format is specific** — does the AI know exactly what shape the answer should take?
- [ ] **Context is sufficient** — does the AI have enough information to start working?
- [ ] **Steps are executable** — can each step be done without asking follow-up questions?
- [ ] **No contradictions** — does any constraint conflict with the goal or another constraint?
- [ ] **Agent-appropriate** — is the format adapted to the target platform?
- [ ] **Verification section present** — does the prompt end with explicit, checkable verification steps the executor MUST run before declaring completion? For delegated/multi-step tasks, this is mandatory. For simple one-shot tasks, a single-line check is sufficient.

**Deep Mode only** — also confirm, and fix the prompt before delivery if any item fails:

- [ ] The Expert Pre-Analysis was printed first, with all five questions, at most five ranked bullets each.
- [ ] Context names primary sources, Steps name methods and tools, Verification names external evidence, and Output Format requires a clear conclusion.
- [ ] Every addition is labeled `addition · 新增` in both the pre-analysis and the prompt. Nothing beyond the user's request is unlabeled.
- [ ] The prompt is deeper, not longer. No line exists only to add length.

---

## 5. ANTI-PATTERNS (What NOT to Do)

- **Do not rewrite the user's intent.** If they say "summarize", don't turn it into "comprehensive analysis". Refine the structure, not the goal.
- **Do not add requirements the user didn't ask for.** No "while you're at it, also..." — that's scope creep.
- **Do not over-structure simple requests.** A one-sentence task sometimes needs only a one-paragraph prompt. Don't force the full template.
- **Do not bury the task.** If the AI has to read 300 words before understanding what to do, the prompt is backwards.
- **Do not ask more than 3 clarifying questions.** If you still have gaps, make reasonable assumptions and flag them: "I assumed X — let me know if that's wrong."
- **Do not deliver without the quality checklist.** A prompt that fails any checklist item MUST be fixed before delivery. Do NOT deliver a prompt with known checklist failures.

**Deep Mode exception.** Only `pc deep` / `pc 深度` deliberately relaxes "Do not add requirements the user didn't ask for." Expert additions are allowed in that mode alone. Each one must appear in the Expert Pre-Analysis and be labeled `addition · 新增` inside the crafted prompt so the user can drop it. A normal `pc` request still follows the bullet above with no exception.

---

## 6. EXAMPLES

### Example 1: Fragmented input → Structured prompt

**User input:**
> i need something for my team's code review. we use github, PRs are messy, people don't leave good comments. want a checklist or something that reviewers can follow. maybe also for the author before they submit. nothing too long.

**Crafted prompt (general-purpose):**

```
## Role
You are a code review quality specialist helping a software engineering team improve their pull request process.

## Task
Create a concise code review checklist for both PR authors and reviewers. The checklist should fit on one printed page and be easy to follow during real review sessions.

## Context
- The team uses GitHub for pull requests.
- Current problem: reviews are inconsistent, comments lack substance, PR descriptions are often incomplete.
- The checklist will be used by both authors (before submitting) and reviewers (during review).

## Steps
1. Design two sections: "Before You Submit" (for authors) and "During Review" (for reviewers).
2. For each section, list 5-8 concrete checklist items. Each item must be a yes/no question.
3. Under each item, add one line explaining why it matters.
4. Add a short "Review Quality Guidelines" section at the end (3-4 principles for writing good review comments).

## Constraints
- Keep the entire document under 500 words.
- No abstract advice like "be thorough" — every item must be checkable.
- Use plain language. No jargon without explanation.

## Output Format
A markdown document with the following structure:
- # PR Checklist
- ## Before You Submit (Author)
  - [ ] Checklist item — why it matters
- ## During Review (Reviewer)
  - [ ] Checklist item — why it matters
- ## Review Quality Guidelines
  - Guideline

## Verification
- Count checklist items in each section — must be 5-8 per section
- Verify total word count is under 500
- Spot-check 3 random items: each must be a yes/no question with a "why it matters" line
```

### Example 2: Claude Code adaptation

**User input:**
> make a claude code prompt that helps me write better commit messages. right now i just write "fix" or "update" and it's useless later.

**Crafted prompt (Claude Code format):**

```
System: You are a commit message assistant embedded in a developer's workflow. Your job is to generate concise, informative commit messages following the Conventional Commits specification. Always use the format: type(scope): description. Types: feat, fix, refactor, docs, test, chore, perf, ci. Keep the first line under 72 characters. Add a body paragraph only when the change is complex.

User: Based on the git diff provided, generate a single commit message.

Steps:
1. Identify the primary change type (feat, fix, refactor, etc.).
2. Determine the scope (the component or module affected).
3. Write a one-line summary under 72 characters.
4. If the change is complex, add a 1-2 sentence body explaining the why, not the what.

Output: Only the commit message. No explanations, no alternatives, no markdown wrapping.

Verification:
- Confirm output is a single line under 72 characters
- Verify format matches: type(scope): description
- Check no markdown wrapping, explanations, or alternatives are present
```

### Example 3: OpenClaw adaptation

**User input:**
> i have a bunch of csv files from different sensors, need to merge them by timestamp and detect gaps. the timestamps are in different formats. openclaw prompt pls.

**Crafted prompt (OpenClaw format):**

```
Goal: Merge multiple CSV files from different sensors into one dataset by timestamp and report any time gaps.

Background: The CSV files come from independent sensors recording at different intervals. Timestamps may be in different formats (ISO 8601, Unix epoch, or MM/DD/YYYY HH:MM:SS). The merged output must be sorted chronologically.

Requirements:
- Auto-detect timestamp format in each file
- Normalize all timestamps to ISO 8601
- Merge by closest timestamp match (tolerance: 1 second)
- Flag rows where no matching timestamp exists within tolerance
- Report any gaps larger than 5 seconds between consecutive rows

Deliverable: A single merged CSV file plus a gap report listing timestamps and duration of each gap.

Constraints:
- Do not drop rows with missing matches; include them with null values in non-matching columns
- Preserve original column names with a file-prefix to avoid collisions (e.g., sensor1_temp, sensor2_temp)
- Handle up to 1 million rows total across all files

Verification:
- Confirm all timestamps in the output are ISO 8601 format
- Verify the merged CSV column count matches (original columns + file-prefixed duplicates)
- Check the gap report lists every gap > 5 seconds with timestamps and duration
- Spot-check 3 random rows: verify merge was correct within 1-second tolerance
```

---

## 7. EDGE CASES

### The user gives almost nothing ("make a prompt for my project")

Ask: "Tell me the one thing the AI should produce. Even one sentence is enough." If the user still cannot answer, this is not a prompt-crafting problem — it's a problem-definition problem. Offer to help them think through it conversationally first.

### The user gives too much (walls of text, multiple topics)

Identify the primary goal. Ask: "It sounds like there are X things here. Which one is the most important right now?" Craft a prompt for that one. Offer to handle the others in separate prompts.

### The user wants a prompt for a task you don't understand

Say so. "I don't know enough about [domain] to craft a reliable prompt for this. Can you point me to a reference or example of what good output looks like?" Do not fake domain expertise.

### The prompt is for a safety-critical or high-stakes task

Add a verification step. Include in the prompt: "Before delivering the final output, verify your work by [specific check]." Add a constraint: "If you are uncertain about any part, flag it explicitly rather than guessing."
