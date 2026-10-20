# code-style

A Claude Code skill with shared coding rules. Load it on any turn that writes,
edits or reviews code.

```bash
git clone https://github.com/A-Eugene/code-style && code-style/install.sh
```

`install.sh` copies the skill into `~/.claude/skills/code-style`. Add `--codex`
to also write it into `~/.codex/AGENTS.md`. For one repo only, copy `SKILL.md`
into that repo's `.claude/skills/code-style/`.
