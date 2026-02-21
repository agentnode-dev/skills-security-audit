# Skill Security Audit

**Detect malicious AI agent skills before they compromise your system.**

A cross-platform security audit skill that scans third-party AI agent skills, plugins, and tool definitions for security vulnerabilities. Uses AI semantic analysis with 61 detection patterns across 9 risk categories aligned with the [OWASP Agentic AI Top 10](https://owasp.org/www-project-agentic-ai-threats/). Zero dependencies -- works on any platform that supports AI agent skills.

---

## Why This Exists

The AI agent skill ecosystem has a security problem. Third-party skills execute with your agent's full privileges, and marketplaces have limited vetting:

- **Snyk ToxicSkills** -- 36.82% of MCP servers on ClawHub have at least one vulnerability
- **SlowMist** -- 472+ malicious MCP servers identified with real-world credential theft and data exfiltration
- **Koi Security ClawHavoc** -- 341 malicious servers found in 2,857 scanned

Skills can read your SSH keys, exfiltrate environment variables, install persistent backdoors, and inject prompts that override your agent's safety guardrails. A single malicious skill compromises everything.

---

## What It Detects

61 detection patterns across 9 categories:

| ID | Category | Severity | OWASP ASI | Patterns |
|----|----------|----------|-----------|----------|
| PI | Prompt Injection | CRITICAL | ASI01 | PI-001 to PI-008 |
| DE | Data Exfiltration | CRITICAL | ASI02 | DE-001 to DE-009 |
| CE | Malicious Command Execution | CRITICAL | ASI02, ASI05 | CE-001 to CE-010 |
| OB | Obfuscated/Hidden Code | WARNING | -- | OB-001 to OB-007 |
| PA | Privilege Over-Request | WARNING | ASI03 | PA-001 to PA-005 |
| SC | Supply Chain Risks | WARNING | ASI04 | SC-001 to SC-006 |
| MP | Memory/Context Poisoning | WARNING | ASI06 | MP-001 to MP-005 |
| TE | Human Trust Exploitation | WARNING | ASI09 | TE-001 to TE-006 |
| BM | Behavioral Manipulation | INFO | ASI10 | BM-001 to BM-005 |

Every pattern includes malicious examples, explanations of the danger, and false-positive guidance. See [`references/security-rules.md`](references/security-rules.md) for the full ruleset.

---

## Quick Start

### 1. Install as a local skill

Copy the skill into your agent's skills directory:

```bash
# Claude Code
cp -r skills-security-audit ~/.claude/skills/

# Or clone directly
git clone https://github.com/agentnode-dev/skills-security-audit.git ~/.claude/skills/skills-security-audit
```

### 2. Audit a specific skill

Ask your AI agent:

```
Audit the skill at /path/to/suspicious-skill/
```

### 3. Scan all installed skills

```
Scan all my installed skills for security issues
```

That's it. No configuration, no API keys, no dependencies.

---

## How It Works

This is a **pure Skill** -- a markdown file that instructs any AI agent how to perform security audits. No code to install, no runtime to configure.

### Phase 1: Determine Scan Scope

The skill identifies target files (`.md`, `.json`, `.js`, `.py`, `.sh`, `.ts`, `.yaml`, `.yml`) from the path you specify, a GitHub URL, or your platform's installed skills directory.

### Phase 2: Analyze Each File

The AI agent reads each file and checks its content against all 61 detection patterns using semantic analysis. Unlike regex-based tools, this catches obfuscated, paraphrased, and novel attack patterns because the AI understands intent, not just string matches.

### Phase 3: Generate Report

A structured report with severity ratings, evidence citations (file path and line number), and actionable remediation for each finding.

---

## Risk Scoring

Each finding contributes to the risk score:

| Finding Severity | Points |
|------------------|--------|
| CRITICAL | +2.0 |
| WARNING | +0.8 |
| INFO | +0.2 |

**Risk levels** (max 10.0):

| Score | Level | Action |
|-------|-------|--------|
| 0.0 -- 2.0 | **SAFE** | No significant risks found |
| 2.1 -- 5.0 | **RISKY** | Manual review recommended before use |
| 5.1 -- 8.0 | **DANGEROUS** | Do not install |
| 8.1 -- 10.0 | **MALICIOUS** | Confirmed malicious intent -- report to marketplace |

---

## Platform Support

This skill works on any platform that supports file-based AI agent skills:

- Claude Code
- ChatGPT
- OpenAI Agents
- Gemini
- Cursor
- Windsurf
- OpenClaw
- ClawHub
- Any platform with AI agent skill support

Because it is pure markdown read by the AI at runtime, there is nothing platform-specific to port or maintain.

---

## Based On

This skill's detection patterns are informed by real-world threat intelligence:

- **[SlowMist](https://slowmist.com/)** -- Analysis of 472+ malicious MCP servers on ClawHub, including two-stage payload loading, file harvesting, and platform relay techniques
- **[Snyk ToxicSkills](https://snyk.io/)** -- Research finding 36.82% vulnerability rate across ClawHub MCP servers
- **[Koi Security ClawHavoc](https://koisecurity.com/)** -- Discovery of 341 malicious servers in 2,857 scanned
- **[OWASP Agentic AI Top 10](https://owasp.org/www-project-agentic-ai-threats/)** -- Framework for categorizing agentic AI security risks (ASI01 through ASI10)

---

## Contributing

Contributions are welcome. To add a new detection pattern:

1. Add the rule to `references/security-rules.md` following the existing format (pattern, malicious example, danger explanation, false-positive guidance)
2. Update the summary table in both `references/security-rules.md` and `SKILL.md`
3. Submit a pull request with a description of the threat the new pattern addresses

---

## License

MIT
