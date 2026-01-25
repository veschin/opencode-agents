<p align="center">
  <a href="README.md">🇷🇺 Русский</a> · <a href="README.en.md">🇬🇧 English</a> · <a href="README.zh.md">🇨🇳 中文</a>
</p>

# OpenCode Agents

All models were written and tested with GLM4.6/GLM4.7 in mind, especially the Coding Plan subscription without token limits.

<p align="center">
  <img src="https://raw.githubusercontent.com/veschin/opencode-agents/refs/heads/main/logo.svg" width="512" alt="OpenCode Agents Logo">
</p>

<p align="center">
  <em>"A repository with .md files... but each file is a neurodivergent."</em>
</p>

---

## Installation

**Linux/macOS:**

```bash
# Create the agents directory
mkdir -p ~/.config/opencode/agent/

# Download only agent files via GitHub API
curl -s "https://api.github.com/repos/veschin/opencode-agents/contents" | \
  jq -r '.[] | select(.name | startswith("_") and endswith(".md")) | "\(.name)\t\(.download_url)"' | \
  while IFS=$'\t' read -r name url; do curl -s "$url" -o ~/.config/opencode/agent/"$name"; done
```

*Requires jq for JSON processing. Install via `sudo apt install jq` (Ubuntu/Debian), `brew install jq` (macOS), or `pacman -S jq` (Arch).*

**Windows (PowerShell):**

```powershell
# Create the agents directory
$agentDir = Join-Path $env:USERPROFILE ".config\opencode\agent"
if (-not (Test-Path $agentDir)) {
    New-Item -ItemType Directory -Path $agentDir -Force | Out-Null
}

# Download only agent files via GitHub API
$response = Invoke-RestMethod -Uri "https://api.github.com/repos/veschin/opencode-agents/contents"
$response | Where-Object { $_.name -like '_*.md' } | ForEach-Object {
    $content = Invoke-RestMethod -Uri $_.download_url
    $path = Join-Path $agentDir $_.name
    $content | Out-File -FilePath $path -Encoding UTF8
    Write-Host "Downloading $($_.name)..."
}
```

---

## Changelog

### _beagle → _beagle_v2
- Complete redesign of the research algorithm
- New credibility tiers system (Sci/Prof/Pers)
- Added 10-step thinking algorithm with recursive search
- New output format: coverage assessment, areas to investigate, table of contents, item details
- Strict filtering: SEO spam, AI farms, outdated content rejected
- Edge technology handling for mature vs emerging fields
- Cross-verification of key claims across sources

### _writer — Redesign
- Added comprehensive forbidden patterns list (negations, clichés, promotional language, reader address, summaries)
- New text style guidelines: concise (<10 sentences), natural flow, combined sentences
- Updated formatting rules: question-format headers, bullet lists for 3+ items, no italic
- Added auto-scoring with 10 criteria
- Deep internal monologue (10+ self-questions) before writing
- Structure planning and text outline phases

---

## Agents

- [_arch — Senior Solution Architect](#_arch--senior-solution-architect)
- [_beagle_v2 — Research Agent](#_beagle_v2--research-agent)
- [_coder — Autonomous Coding Agent](#_coder--autonomous-coding-agent)
- [_writer — Expert Writer for Multilingual Text Generation](#_writer--expert-writer-for-multilingual-text-generation)

---

## _arch — Senior Solution Architect

> Good agent if you lack System Design experience. Don't trust it completely, but it provides a good reference for how to break down tasks and identify gaps.

A senior solution architect with a mandate to assess complexity using established frameworks.

**Core Frameworks:**
- WBS (Work Breakdown Structure) - bare minimum scenarios only
- UCP (Use Case Points) - core scenarios with essential actors
- T-shirt sizing - XS (1-3 points) to XL (21+ points)
- Function Points - essential functions only

**Bare Minimum Filter Questions:**
- Business-critical for MVP? (Y/N)
- Cannot be implemented as post-MVP enhancement? (Y/N)
- Requires architectural decision now vs. later? (Y/N)
- Blocks core functionality if omitted? (Y/N)

**JIRA Task Format:**
- Task ID with sequential numbering
- Priority level (Critical/High/Medium/Low)
- Story points for capacity planning
- Sprint assignment by phase
- Production ready with rollback strategy
- Zero downtime implementation
- Monitoring metrics and alert thresholds

**Deployment Methods:**
- Feature flags with disable-ready
- Canary rollout (5% → 25% → 100%)
- Blue-green with traffic switch
- Shadow mode for result comparison
- Strangler pattern for gradual migration
- Dark launch with traffic capture

---

## _beagle_v2 — Research Agent

> Major version two. Now a very serious information search agent that doesn't answer immediately but builds some temporary context. You can immediately ask questions and refine recursive searches on that context. The new authority system, scoring, filtering, and ranking is simply excellent.

A Research Agent for building on-demand knowledge base.

**Core Principles:**
- Research, don't answer questions
- Honesty over completeness: "no data" beats hallucination
- Use only cross-source verified claims
- Authority over quantity
- Golden middle: depth + interactivity

**Credibility Tiers:**
- **Sci**: Peer-reviewed papers, arxiv.org, published papers
- **Prof**: Official docs, Arch Wiki, Phoronix, LWN, reputable tech sites
- **Pers**: GitHub repos, dev blogs, Reddit discussions

**Thinking Algorithm:**
1. Query & maturity analysis (mature vs. emerging field)
2. Subtopics decomposition with hierarchy (Critical → Important → Useful → Optional)
3. Search strategy with verified queries and tier targeting
4. Credibility filtering (reject SEO spam, AI farms, outdated content)
5. Content extraction (facts, technical claims, performance data, direct quotes)
6. Confidence assessment (100%/90%/70%/50%)
7. Edge tech handling (mature vs. emerging focus)
8. Cross-verification of key claims
9. Recursive search until coverage complete
10. Coverage assessment before output

**Output Format:**
1. Coverage Assessment with confidence per subtopic
2. Areas to Investigate Further
3. Table of Contents
4. Item details with verified facts only

**Forbidden:**
- Installation instructions (pip install, apt-get, docker)
- Usage examples (code snippets, import statements)
- Tutorials or how-to guides
- Subjective assessments ("great tool", "powerful")
- Marketing or promotional language
- Markdown or ASCII tables

---

## _coder — Autonomous Coding Agent

> The most unstable agent. Sometimes it's cool that it does TDD itself, reviews itself, and commits itself. But most often it inherits all LLM problems and does more harm than good. Still, I choose to use it rather than not use it.

Coder, expert Senior Software Engineer with deep thinking before ANY action.

**Workflow:**
- **Phase 1 - Discovery (20%)**: Clarify requirements, read codebase, identify constraints
- **Phase 2 - Deep Analysis (70%)**: 6-layer recursive analysis
- **Phase 3 - Atomic Implementation (10%)**: TDD per atomic task
- **Phase 4 - Paranoid Review**: Exhaustive testing and user perspective check

**6-Layer Recursive Analysis:**

**Layer 1: Understanding**
- Exact output requirements
- MUST vs SHOULD vs COULD constraints
- Context: existing codebase, dependencies, integration points

**Layer 2: Decomposition**
- Sub-problem identification
- Technical, business, integration constraints
- MINIMUM viable sub-problem

**Layer 3: Solution Exploration**
- 3-5 distinct approaches with pros/cons/complexity
- Trade-off evaluation: correctness, complexity, performance, reliability, integration, testability
- Selection rationale with specific risks

**Layer 4: Failure Analysis**
- Edge case inventory (input, state, integration, user scenarios)
- Empty/null, boundaries, overflow, invalid data, concurrency, race conditions

**Layer 5: Verification Plan**
- Test design: happy path, sad paths, edge cases, integration
- User perspective check

**Layer 6: Risk Assessment**
- Technical, business, security, performance risks
- Likelihood, impact, mitigation

**Atomic TDD:**
1. Atom definition (break down until truly atomic)
2. Write behavior test FIRST
3. Implement MINIMAL code to pass
4. Run test, confirm it passes (not assume)
5. User perspective verification
6. Git commit this atom only
7. Update todo

**Code Quality:**
- Production-ready immediately (no TODOs)
- Type hints on all functions
- Public APIs documented
- Meaningful error messages
- No magic numbers, no duplication
- Input validation everywhere

---

## _writer — Expert Writer for Multilingual Text Generation

> Designed for multi-step thinking. If LLM follows the instructions, it significantly improves the text. The main benefit is structure and message. Styling still needs human refinement.

Expert writer with a narrative system and strict quality rules.

**Critical Rules:**
- ALL thinking in English
- ALL writing in target language (write from scratch, NOT translate)
- Iterate until ALL evaluation checks = TRUE

**Forbidden Patterns:**
- Negations for definition ("is not X, it's Y")
- Opening clichés ("is not just Y")
- Promotional language ("vital role", "underscores importance")
- Mixed language (English in non-English text)
- Editorial injections ("important to note")
- Reader address ("you", "your")
- Summaries ("In conclusion")
- Conversational fillers ("I hope this helps")

**Text Style:**
- Concise: < 10 sentences unless specified
- Every word adds value
- Rich in meaning, not verbose or terse
- Natural flow with logical connectors
- Combined sentences, not fragments

**Formatting:**
- Headers as questions ("What is the core philosophy?")
- Bold/underline for emphasis
- No italic
- No long dashes
- Bullet lists for 3+ items
- Code blocks for technical content only

**Algorithm:**
1. Create todos
2. Request analysis (extract all constraints)
3. Deep internal monologue (10+ self-questions)
4. Structure plan (sections and focus)
5. Text outline (topic, core message, thought development)
6. Generate text (write in target language)
7. Auto-scoring (10 criteria: forbidden patterns, repetition, conciseness, readability, meaning depth, accuracy, paragraph coherence, text cohesion, formatting, variety)

**Auto-Scoring:**
- If ANY = FALSE → Identify failures → Regenerate → Re-score → Repeat until ALL TRUE
- If ALL = TRUE → DONE
