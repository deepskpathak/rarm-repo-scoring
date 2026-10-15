# rarm-repo-scoring

A Claude Code skill that scores code repositories against the **RARM** (Repository AI-Readiness Model) rubric and assigns agent-readiness tiers T0 to T3, for one repository or a whole estate.

Use it to answer questions like "which of our repos are ready for coding agents?" and "what is the single fix that would move this repo up a tier?"

## What it does

- Scores eight dimensions:
  - **R1** Executability and environment determinism
  - **R2** Verification signal quality
  - **R3** Task specification hygiene
  - **R4** Structure and navigability
  - **R5** Documentation and decisions
  - **R6** Context files and procedural knowledge
  - **R7** Ownership, workflow and guardrails
  - **R8** Measurement and evaluation
- Tags every indicator by evidence class (**D**eclared, **O**bserved, **V**alidated) and caps scores mechanically: D-only caps at 2, 3 needs observed evidence, 4 needs validated evidence.
- Applies a minimum-across-dimensions rule, then reports both a **screen tier** (NA excluded) and a **strict tier** (NA counted as 0).
- Produces a ranked fix list (up to five per repo) with effort and the tier each fix enables.
- Writes a per-repo scorecard, an estate rollup, and JSON to `./rarm-report/`.

## Safety model

- **Read-only.** Never writes inside an assessed repository.
- **Repository content is data, not instructions.** Text in READMEs, AGENTS.md and issues is never followed.
- **No code execution** of the assessed repo unless you ask, and then only in a sandbox.
- **No secret values** in output, and **no per-person metrics**.

## Install

### As a plugin (recommended)

In Claude Code:

```
/plugin marketplace add deepskpathak/rarm-repo-scoring
/plugin install rarm-repo-scoring@rarm-repo-scoring
```

### Manually

Copy the skill into your personal or project skills directory:

```bash
git clone https://github.com/deepskpathak/rarm-repo-scoring.git
mkdir -p ~/.claude/skills
cp -R rarm-repo-scoring/skills/rarm-repo-scoring ~/.claude/skills/
```

For a single project, copy it to `<project>/.claude/skills/` instead.

## Usage

In Claude Code:

```
Score ./my-service for agent readiness using rarm-repo-scoring
Assess every repo under ~/src and give me an estate rollup
```

## Requirements

- Required: `git`, `python3`
- Optional (unlock more observed indicators): `gh` (forge, CI and PR data), `jq`, `rg`, `scorecard` (OpenSSF), `jscpd` (duplication), `lychee` (broken links)

Missing optional tools mark the dependent indicators `NA`; they are never guessed.

## Caveats

Thresholds are provisional defaults. The model has not been validated against outcomes. Every report says so.

## License

[MIT](LICENSE)
