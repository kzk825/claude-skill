# CLAUDE.md — AI Assistant Guide for claude-skill

This file provides context for AI assistants (Claude, Copilot, etc.) working in this repository.

---

## Repository Overview

This is a **content operations management repository** for AI-driven content creation and distribution. It provides reusable AI skills, prompt templates, and automation workflows targeting Japanese-speaking AI users (beginners to intermediate).

**Primary Goal:** Automate the pipeline from AI news collection → summarization → SNS posting → article creation, reducing content distribution work time by ~80%.

**Brand identity:** くろ🦉 — warm, sincere, practical tone. AI + time-saving focused.

---

## Repository Structure

```
claude-skill/
├── .github/workflows/
│   └── content-check.yml        # CI: validates skill/prompt files via Python
├── check_prompts.py             # Content validation script (runs in CI)
├── docs/
│   └── usage-guide.md           # How to use skills and run workflows
├── product/
│   └── vision.md                # Product strategy and goals
├── project/
│   └── PROJECT_INSTRUCTIONS.md  # Brand rules, content style guide (critical)
├── prompts/                     # Standalone prompt templates
│   ├── ai-news-pipeline.md
│   ├── daily-reflection.md
│   └── x-post/
│       └── sample-x-post-prompt.md
├── skills/                      # Reusable AI skills
│   ├── daily-reflection.md
│   ├── 投稿｜X投稿文作成スキル  # X post generation skill
│   ├── 要約│ニュース要約スキル  # News summarization skill
│   ├── Skill: dispatche         # Skill dispatcher/router
│   ├── 図解イラスト作成/
│   │   └── SKILL.md             # Infographic generation skill
│   └── 投稿│トレンド/
│       └── SKILL.md             # AI trend summarization skill
├── templates/
│   ├── skill-template.md        # Boilerplate for new skills
│   └── workflow-template.md     # Boilerplate for new workflows
└── workflows/                   # Multi-step automation workflows
    ├── ai-news-pipeline.md
    ├── ai-news-to-x-post.md
    └── claude-make-automation.md
```

---

## Development Workflows

### Validation

There is no build system. The primary development action is running the content validation script:

```bash
python check_prompts.py
```

This script scans `skills/`, `prompts/`, and `project/` directories and checks:

- **Errors (fail CI):**
  - `project/PROJECT_INSTRUCTIONS.md` must contain: `## 目的`, `## トーン・文体`, `## 出力ルール`, `## 禁止事項`
  - Files in `prompts/*.md` must contain: `## 目的`, `## 出力条件`

- **Warnings (print but do not fail):**
  - Missing H1 (`#`) header
  - File size > 6000 characters
  - No citation / URL / reference information
  - Typography inconsistencies (e.g., "Git hub" instead of "GitHub", "chatgpt" instead of "ChatGPT")
  - Forbidden expressions: "神すぎる", "やばい"

### CI/CD

GitHub Actions runs automatically on push when any of these paths change:

- `skills/**/*.md`
- `prompts/**/*.md`
- `project/**/*.md`
- `check_prompts.py`

The workflow uses Python 3.11 and exits non-zero on validation errors.

---

## Key Conventions

### Language

- **All skill/prompt content must be written in Japanese.** This is a hard rule from `PROJECT_INSTRUCTIONS.md`.
- Code, scripts, config files, and this `CLAUDE.md` use English.
- File and directory names may use Japanese (e.g., `図解イラスト作成/`).

### Content Style (Brand Voice)

Defined in `project/PROJECT_INSTRUCTIONS.md`. Key rules:

- **Tone:** Warmth + sincerity + practicality. Friendly but not superficial.
- **Priority order:** Accuracy > Readability > Practicality > Reusability > Brand consistency
- **Avoid:** Hyperbole, unsourced claims, excessive positivity, informal slang ("神すぎる", "やばい")
- **X posts:** Target ~180 characters standard, max ~220
- **Sources:** Always cite sources; mark unknown sources as `出典不明`
- **Brand emojis:** 🦉 📡 ⚡️ 🧠 🛠️ 📝 🤖 💼 💬 📌

### Skill File Structure

New skills should follow this pattern (see `templates/skill-template.md`):

```markdown
# Skill: [Name]

## トリガー
[Keywords/phrases that activate this skill]

## 役割
[What this skill does]

## [Content Sections]
[Implementation details]

## 出力ルール / 出力形式
[How output should be formatted]
```

### Workflow File Structure

New workflows should follow `templates/workflow-template.md`. Workflows chain multiple skills together into a pipeline.

### Infographic Design Rules (skills/図解イラスト作成/SKILL.md)

This skill has strict design constraints:

- **Priority:** Readability >> Aesthetics >> Decoration
- **Colors:** White, off-white, warm beige, soft pastels — **no dark/neon/cyberpunk themes**
- **Layout:** 3-6 content blocks, generous whitespace, paper-like aesthetic
- **Text:** Large and readable, 1-2 lines per block, max 10 items per composition
- **Aspect ratios:** 9:16 (vertical/SNS), 16:9 (landscape), 5:2 (header banner)

---

## File Naming

| Type | Convention | Example |
|------|-----------|---------|
| Skills | Japanese descriptive name | `投稿｜X投稿文作成スキル` |
| Skill directories | Japanese topic + `/SKILL.md` | `図解イラスト作成/SKILL.md` |
| Prompts | Kebab-case English | `ai-news-pipeline.md` |
| Workflows | Kebab-case English | `ai-news-to-x-post.md` |
| Templates | Kebab-case English | `skill-template.md` |

---

## Adding New Content

### Adding a New Skill

1. Copy `templates/skill-template.md` to `skills/` with a Japanese filename
2. Fill in all required sections (role, triggers, output format)
3. Ensure content follows brand guidelines in `project/PROJECT_INSTRUCTIONS.md`
4. Run `python check_prompts.py` to validate
5. Commit — CI will validate automatically

### Adding a New Prompt

1. Create a `.md` file under `prompts/`
2. **Required sections:** `## 目的` and `## 出力条件`
3. Run `python check_prompts.py` to validate

### Adding a New Workflow

1. Copy `templates/workflow-template.md` to `workflows/`
2. Document each step and the skills it calls
3. No automated validation beyond CI — focus on clarity

---

## Git Configuration

- **Commit signing:** SSH-based signing is enabled (`gpg.format=ssh`)
- **Main branch:** `main`
- **Feature branches:** Use descriptive names (e.g., `claude/add-claude-documentation-x0uvo`)
- No custom git hooks are installed

---

## What This Repository Is Not

- Not a compiled application — there is no `npm`, `pip install`, or build step
- Not a Python package — `check_prompts.py` is a standalone script
- Not an API service — workflows describe automation patterns, not deployed code
- Not English-first — all user-facing content is Japanese

---

## Critical Files to Read First

If working on this repository, read these files in order:

1. `project/PROJECT_INSTRUCTIONS.md` — brand rules and content constraints (highest authority)
2. `product/vision.md` — product strategy and goals
3. `docs/usage-guide.md` — how skills and workflows are used
4. `check_prompts.py` — understand what the validator enforces
