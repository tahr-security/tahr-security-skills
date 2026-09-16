# Tahr Security Skills

Pentester-informed skills that help developers review applications with an attacker's mindset, validate exploitability, and avoid false confidence from shallow LLM or scanner output.

The suite emphasizes evidence, authorization boundaries, realistic attacker outcomes, explicit coverage, and honest uncertainty. It is intended for applications and environments you own or are explicitly authorized to test.

## Install

Install the complete suite:

```bash
npx skills add tahr-security/tahr-security-skills
```

Install one skill:

```bash
npx skills add tahr-security/tahr-security-skills --skill tahr-secure-app
```

Start with `tahr-secure-app` for a coordinated application review. Use a specialist skill directly when the review scope is already narrow.

## Skills

| Skill | Purpose |
| --- | --- |
| `tahr-secure-app` | Coordinate a comprehensive, evidence-backed application security review. |
| `tahr-map-attack-surface` | Build a role-aware inventory of reachable routes, parameters, interfaces, and trust boundaries. |
| `tahr-test-authentication` | Review login, recovery, MFA, sessions, tokens, SSO, and account-takeover paths. |
| `tahr-test-access-control` | Model and trace object-, function-, property-, role-, and tenant-level authorization; prove exploitable gaps and reject weak ID/status-code leads. |
| `tahr-trace-dangerous-inputs` | Trace attacker-controlled input to injection, SSRF, traversal, upload, and browser sinks. |
| `tahr-test-business-workflows` | Abuse-test stateful workflows, invariants, races, replay, quotas, and entitlements. |
| `tahr-audit-secrets-config` | Audit secrets, cryptography, dependencies, CI/CD, infrastructure, and runtime configuration. |
| `tahr-test-ai-agents` | Test LLM, RAG, memory, ingestion, rendering, tool-calling, MCP, and agent boundaries. |
| `tahr-audit-android` | Review Android artifacts, exported components, IPC, WebViews, storage, and mobile APIs. |
| `tahr-threat-model-app` | Threat-model an entire existing application with connected attack paths, security decisions, validated coverage, and executable test handoffs. |
| `tahr-verify-security-fix` | Retest a remediation in the original vulnerable context and check for bypasses and regressions. |
| `tahr-review-tahr-findings` | Optionally review Tahr account applications, assessments, and findings through a read-only workflow. |

`tahr-review-tahr-findings` is an optional customer skill that requires a manually configured Tahr MCP connection in Codex and keeps all Tahr interactions read-only. All other skills work without Tahr or a Tahr account. Configure credentials through the Codex MCP connection; never paste a token into chat.

## Design principles

- Treat findings as hypotheses until evidence proves reachability and impact.
- Separate observations, candidates, confirmed findings, and untested coverage.
- Test horizontally across users, roles, tenants, states, parsers, and equivalent interfaces.
- Prefer reversible, minimally harmful proof in authorized local or staging environments.
- Never turn a clean result into a claim that an application is secure.

## Repository layout

Each directory under [`skills/`](skills/) is an independently installable skill. Every skill contains a `SKILL.md` definition and may include focused references, scripts, assets, and agent metadata.

```text
skills/
  tahr-secure-app/
    SKILL.md
    agents/openai.yaml
    references/
    scripts/
    assets/
```

## Contributing

Keep a skill focused and evidence-driven. Before proposing a change:

1. Validate its `SKILL.md` metadata and referenced files.
2. Exercise the workflow against a representative fixture or application.
3. Confirm that reporting language distinguishes confirmed impact from candidates and coverage gaps.
4. Avoid destructive testing instructions, credential exposure, persistence, or activity outside explicit authorization.

## License

This repository is licensed under the [GNU General Public License version 3.0 only](LICENSE) (`GPL-3.0-only`).
