<!-- Personal_Rules_START -->

## Personal-Rules

- Please answer in Chinese(中文). Regardless of the language of the question, respond entirely in Chinese(中文).
- The current machine environment is Ubuntu 24.04 on WSL2.

<!-- Personal_Rules_END -->

<!-- Karpathy_Guidelines_START -->

## Karpathy-Guidelines

Behavioral guidelines to reduce common LLM coding mistakes. Merge with project-specific instructions as needed.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

### 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:

- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

### 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

### 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:

- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:

- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

### 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:

- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:

```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.

<!-- Karpathy_Guidelines_END -->

<!-- Grilling_Protocol_START -->

## Grilling-protocol

> Whether or not [superpower](https://github.com/obra/superpowers) — "Reusable Skills and Development Methodology Framework" or [comet](Reusable Skills and Development Methodology Framework) — "Long Task Flow Execution, Skill Orchestration and Scientific Evaluation Platform" is utilized, **the Grilling-protocol must be strictly followed**.

When the user proposes a plan, design, or refactor whose intent is not yet clear, do not act — clarify via the "decision-tree grilling" protocol:

- **Look it up first**: Factual questions (code locations, API behavior, library versions, config keys) — resolve them yourself via grep/Read/Context7/webfetch. Never surface a fact as a question to the human.
- **One question at a time, with a recommendation**: Each question must include your own recommended answer, so the user can resolve it with "agree / modify / reject". Never batch multiple questions.
- **Exhaust every branch**: Treat the fuzzy goal as an if-else decision tree. Walk each branch to its boundary conditions — no skipping, no parallels.
- **Stop on contradiction**: If the user's description conflicts with the code, docs, or surrounding context, surface the conflict immediately. Do not continue on a false premise.
- **No code until aligned**: Until the user explicitly says "start" / "go", do not write code, execute plans, or create todo lists — keep grilling.

<!-- Grilling_Protocol_END -->

<!-- Sensitive_Information_Handling_START -->

## Sensitive-Information-Handling

API key / token / secret — Never hardcode them into source code or commit content:

- No local storage: Retrieve them from environment variables (`$API_KEY`) or secret managers; add `.env` / `.env.*.local` to `.gitignore`, and submit `.env.example` as a template.

- No leakage: Redact API keys in all documents, demos, logs, commit messages and PR descriptions (use `sk-***` in outputs); use `<your-api-key>` as placeholders in examples and fixtures; never paste real keys in chats or files.

- No full exposure: Only display the first 4 characters followed by `***` for secrets in logs; full values must never be printed.

<!-- Sensitive_Information_Handling_START -->

<!-- meisijiya-extras_START -->

## Agent project layout (when starting agent/LLM projects)

If starting an agent/LLM project (not a regular web app / CLI tool), use this 10-folder skeleton under `agent-project/`:

- `01_planner/` — goals, plans, roadmaps, task breakdown
- `02_memory/` — long/short/entity memory + knowledge graph
- `03_tools/` — tool descriptions (api / code / custom)
- `04_knowledge/` — RAG, embeddings, vector store
- `05_agent_core/` — agent role, persona, system prompt, workflow
- `06_evaluation/` — metrics, test cases, benchmarks
- `07_observability/` — logs, traces, monitoring
- `08_deployment/` — docker, k8s, env
- `09_examples/` — use cases, demo flows
- `10_docs/` — architecture, best practices, FAQ

Complements pwf (which handles `task_plan.md` / `findings.md` / `progress.md`). Don't apply to non-agent projects.

## omo features → meisijiya-skills (quick map)

Leverage these omo features directly instead of reinventing:

- **MCPs** (use as tools): `mcp__context7__*` for library docs, `mcp__grep_app__*` for GitHub code search, `mcp__websearch__*` for current info, `mcp__lsp__*` for goto-definition
- **Agents** (delegate via Sisyphus): `prometheus` (interview-mode planning), `atlas` (todo orchestration), `oracle` (architecture/debug — read-only), `explore`/`librarian` (search), `multimodal-looker` (vision)
- **Built-in skills** (invoke directly): `git-master`, `frontend-ui-ux`, `playwright`/`agent-browser`, `review-work`, `remove-ai-slops`, `ast-grep`, `init-deep`
- **Modes**: `ulw` (auto-all), `hyperplan` (5-agent adversarial review), `security-research` (3 hunters + 2 PoC), `team` (parallel)
- **Categories**: `visual-engineering` (UI), `ultrabrain` (hard logic), `deep` (multi-step), `quick` (trivial edits), `writing` (docs)

Full details: see `omo-integration` (was a skill, now consolidated here).

<!-- meisijiya-extras_END -->

<!-- meisijiya-skills:start -->

## meisijiya-skills

Use this skill system for the omo + pwf stack. Invoke skills by matching their `description` field against the user's request; do not invoke skills that don't match.

These conventions apply globally unless a project-level AGENTS.md overrides them.

### Catalog

**.core/ — load always:**

- `using-meisijiya-skills` — meta dispatcher; check before every response
- `spec-driven-development` — spec before non-trivial code
- `incremental-implementation` — vertical slices (≤ 100 lines each)
- `test-driven-development` — red-green-refactor
- `debugging-and-error-recovery` — 5-step triage (reproduce / localize / reduce / fix / guard)
- `source-driven-development` — verify API against official docs

**.extra/ — load on demand:**
`pwf-enforcer` · `build-gate-visual-review` · `designer-handoff` · `interview-me` · `code-simplification` · `api-and-interface-design` · `security-and-hardening` · `performance-optimization` · `observability-and-instrumentation` · `documentation-and-adrs`

### omo integration

For the reverse map (omo feature → skills that use it), see the `meisijiya-extras` block above. Skills use:

- `source-driven-development` — context7 MCP (primary), grep_app MCP
- `debugging-and-error-recovery` — oracle agent (escalation), lsp MCP
- `incremental-implementation` — git-master skill, atlas agent
- `designer-handoff` — visual-engineering category, frontend-ui-ux skill
- `security-and-hardening` — security-research mode
- `using-meisijiya-skills` — Sisyphus (executing delegation), atlas (todo orchestration)

### Conventions

- Don't ship code without spec + tests

[Injected by meisijiya-skills/scripts/inject-agents-md.sh — do not edit]
<!-- meisijiya-skills:end -->
