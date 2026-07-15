---
name: tahr-threat-model-app
description: Build an implementation-backed application threat model with assets, actors, trust boundaries, data flows, abuse cases, attack paths, security invariants, control gaps, and executable validation tests. Use for architecture or feature threat modeling, design reviews, pre-pentest planning, API and GraphQL risk analysis, privacy reviews, AI/LLM workflows, or when source, specifications, diagrams, and deployment documents must be correlated.
---

# Tahr Threat Model App

Use source as evidence of observed implementation and use specifications and
documents as evidence of intent. Produce a decision-oriented threat model, not
a list of isolated code smells and not a verified vulnerability report.

## Set scope and evidence classes

1. Record the repository revision, included packages, deployment environments,
   supplied documents/specifications, exclusions, and unanswered questions.
2. Label every material statement as `observed`, `intended`, `inferred`, or
   `unknown`. Do not let documentation or a framework convention prove runtime
   behavior.
3. Default to read-only source and document analysis. Use a runtime target only
   with explicit authorization for that target and safe test objective.
4. Redact credentials, tokens, private keys, personal data, and customer data.
   Preserve only the minimum non-secret evidence needed for traceability.

Read [threat-model-ledgers.md](references/threat-model-ledgers.md) before
building the model. Use its inventory, flow, risk, control, test, and coverage
records throughout the work.

Keep threat candidates, supporting evidence/proof records, and coverage
accounting separate. A catalog match can suggest a threat, evidence can support
or contradict it, and coverage can show it was reviewed; none substitutes for
the others.

## Build the architecture inventory

Inspect source, manifests, IaC, configuration, API/GraphQL schemas, diagrams,
role matrices, workflows, integration notes, and deployment documents. Map:

- actors and system principals, including unauthenticated, user, peer,
  cross-tenant, admin, service, worker, and third-party identities;
- critical business and security assets;
- logical components, trust zones, data stores, caches, queues, external
  integrations, AI providers, tools, and control-plane surfaces;
- REST, GraphQL, RPC, socket, webhook, job, CLI, upload/download,
  import/export, serverless, and admin entrypoints;
- authentication, session, authorization, ownership, tenant, validation,
  secrets, logging, and rate/usage control placement.

Group related endpoints into capabilities and workflows. Preserve route and
file evidence only where it explains a component, flow, boundary, control, or
security decision.

## Trace attacker-relevant data flows

For every sensitive or state-changing flow, record:

- actor and input source;
- entrypoint and component handoffs;
- assets and data classification;
- each trust-boundary crossing;
- identity, owner, tenant, role, policy, validation, and serialization decision;
- data store, outbound integration, background worker, renderer, or other sink;
- response, side effect, or durable state;
- observed, intended, assumed, and missing controls.

Follow asynchronous continuation and thin service handoffs. Do not stop at the
HTTP controller when a worker, webhook, repository, or tool performs the
security-sensitive action.

## Correlate intent with implementation

Compare source-discovered behavior with API specifications, diagrams, role
matrices, deployment boundaries, workflows, and control statements. Record:

- code-only and spec-only interfaces;
- documented controls not located in source;
- observed controls absent from design documents;
- ambiguous owner, tenant, role, or control ownership;
- undocumented boundary crossings and third-party trust;
- diagram, topology, deployment, and workflow drift.

Convert unverified intent into an assumption, limitation, security decision,
or validation test. Never convert it directly into a finding.

## Derive threats and security decisions

For each material asset and flow:

1. State the security invariant, such as “tenant scope precedes export” or
   “webhook authenticity is verified before state change.”
2. Describe a concrete actor goal, preconditions, boundary crossed, abuse path,
   affected asset, and business impact.
3. Identify visible, assumed, missing, and potentially bypassable controls.
4. Search for evidence that contradicts the modeled gap.
5. Rank the risk using impact, exposure, asset sensitivity, privilege,
   preconditions, control evidence, and coverage confidence.
6. Define the design decision and a validation test that would resolve the
   uncertainty.

Use STRIDE as a completeness checklist, then retain only implementation-relevant
threats. Map relevant decisions to ASVS, OWASP API, or GraphQL themes without
treating those catalogs as evidence.

Apply [threat-evidence-and-quality-gates.md](references/threat-evidence-and-quality-gates.md)
before finalizing risks or tests.

## Add conditional analysis

- When personal or sensitive data is present, examine linkability,
  identifiability, disclosure, unawareness, and compliance/consent boundaries.
- When LLMs, agents, RAG, embeddings, MCP, prompts, or tools are present,
  examine direct and indirect prompt injection, authorization loss at tool or
  retrieval boundaries, data exfiltration, supply-chain trust, evaluation
  bypass, and wallet/quota abuse.
- Mark either lane not applicable with evidence rather than inventing risks.

## Produce executable validation cases

For every material uncertain or missing control, specify the actor, asset,
boundary, preconditions, fixture/setup, exact test objective, expected control,
positive success signal, negative/denial signal, evidence to collect, safe
target environment, and confidence. These are test hypotheses, not confirmed
vulnerabilities.

## Review model quality and coverage

Check that every high-signal entrypoint, auth/authz file, sensitive asset,
trust boundary, integration, worker, admin surface, deployment zone, and
security decision is read or dispositioned. Record unread items and their
confidence impact.

Do not publish a clean or complete threat model while high-risk coverage gaps,
unknown trust boundaries, or unowned critical controls remain unresolved. Mark
the model `incomplete_high_risk_coverage`, preserve the gaps, and prioritize
the work needed to close them.
