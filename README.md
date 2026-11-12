# Shared Agent Skills

A public repository of reusable, portable [Agent Skills](https://agentskills.io) for AI coding assistants (including Google Antigravity, GitHub Copilot, Claude Code, and other Agent Skills-compatible hosts).

Each skill lives in its own top-level directory containing a `SKILL.md` instruction manifest along with optional helper scripts, reference guides, and unit tests.

---

## Available Skills

| Skill | Description |
| :--- | :--- |
| [`trivy-remediation`](./trivy-remediation/) | Scan a Git project with Trivy, inventory vulnerabilities, misconfigurations, and secrets, and remediate findings sequentially with regression tests and mandatory human review before merging. |

---

## Installation & Usage

### Option 1: Reference via `skills.json` (Antigravity)

Clone this repository locally and register it in your global `~/.gemini/config/skills.json` or project-local `.agents/skills.json`:

```json
{
  "entries": [
    {
      "path": "/path/to/skills"
    }
  ]
}
```

Or inherit from the repository's [`skills.json`](./skills.json) manifest (optionally filtering with `include_only` or `exclude`):

```json
{
  "inherits": [
    {
      "path": "/path/to/skills/skills.json"
    }
  ]
}
```

### Option 2: Copy a Single Skill

Copy any skill directory (e.g., `trivy-remediation/`) into your host's skill discovery location:

- **Project-scoped**: `<project>/.agents/skills/<skill-name>/`
- **Antigravity global**: `~/.gemini/config/skills/<skill-name>/`
- **GitHub Copilot personal**: `~/.copilot/skills/<skill-name>/` or `~/.agents/skills/<skill-name>/`
- **Claude Code**: `~/.claude/skills/<skill-name>/` or `<project>/.claude/skills/<skill-name>/`

---

## Repository Structure

```text
.
├── skills.json                  # Root manifest for Antigravity skill inheritance
├── trivy-remediation/           # Trivy vulnerability/misconfig/secret remediation skill
│   ├── README.md                # Skill-specific usage and requirements
│   ├── SKILL.md                 # Agent instructions and frontmatter
│   ├── references/              # Detailed remediation and reporting references
│   ├── scripts/                 # Supporting workflow helper scripts
│   └── tests/                   # Unit tests for workflow helpers
└── .github/workflows/ci.yml     # Automated validation and test runner
```

---

## Authoring a New Skill

1. Create a new top-level directory named with lowercase hyphenated words (e.g., `my-new-skill/`):
   ```text
   my-new-skill/
   ├── SKILL.md          # Required: Main instruction file with YAML frontmatter
   ├── README.md         # Recommended: Human-facing overview and requirements
   ├── scripts/          # Optional: Executable helpers
   ├── references/       # Optional: Detailed reference docs loaded on demand
   └── tests/            # Optional: Unit tests (test_*.py)
   ```
2. Begin `SKILL.md` with YAML frontmatter where `name` matches the directory name and `description` states what the skill does and when the agent should activate it:
   ```markdown
   ---
   name: my-new-skill
   description: >-
     Describe what this skill does and when the agent should use it.
   ---
   ```
3. Keep `SKILL.md` focused and concise, delegating detailed reference material to `references/` for progressive disclosure.
4. Add unit tests under `<skill-name>/tests/` for any scripts in `<skill-name>/scripts/`.
5. Run validation and unit tests locally before committing:
   ```bash
   python3 -B -m unittest discover -s <skill-name>/tests -v
   ```

---

## License

This repository is licensed under the [MIT License](./LICENSE).
