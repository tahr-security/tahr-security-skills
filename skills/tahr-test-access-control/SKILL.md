---
name: tahr-test-access-control
description: Model, review, and safely test object-, function-, property-, role-, and tenant-level authorization in web applications, REST APIs, and GraphQL. Use for IDOR/BOLA/BFLA reviews, multi-user or multi-tenant isolation testing, admin and role boundary analysis, ownership checks, mass-assignment authorization review, or validating access-control findings.
---

# Test Access Control

Prove who acted on what, who should control it, why access should be denied, and what unauthorized impact occurred.

## Establish authority and safety

1. Identify the repository, application/API origins, roles, tenants, supplied identities, and expected collaboration model.
2. Do not send runtime requests unless the target and testing are authorized. Perform source/spec/traffic modeling when runtime authorization is absent.
3. Treat supplied accounts and existing customer objects as protected. Use them as callers for read-only checks only.
4. Require fresh disposable objects for write, delete, ownership, role, invitation, billing, credential, or lifecycle mutations. Define readback, restoration, and cleanup before sending the mutation.
5. Redact raw credentials, tokens, cookies, private object contents, and unrelated PII. Preserve actor labels, object/tenant attribution, response hashes, field classes, and short redacted snippets.

## Establish identity truth

Bind each runtime session to its observed user, configured role, tenant, and transport. Keep configured claims separate from observed claims. Do not infer identity from a filename, token claim, role label, or current-user field alone.

Mark an identity `trusted`, `coverage_only`, or `unusable`. A stale, failed, mismatched, or browser-bound session used through the wrong transport is a coverage limitation, not an access denial.

## Build the authorization model

Before replaying requests, record:

- subjects: unauthenticated, user, same-role peer, higher/lower role, service actor, and cross-tenant actor;
- resources: type, identifier carriers, owner, tenant, sensitivity, lifecycle, and safe disposable status;
- actions: create, read, list, update, delete, approve, invite, export, transfer, change role/owner, and workflow transitions;
- context: relationship, workflow state, feature/plan, organization, API version, content type, and authentication strength;
- expected rules: allow, deny, or unknown, each citing code, documentation, schema, UI behavior, owner baseline, or explicit user context.

Do not invent expected-denial rules from endpoint names. Resolve unknown collaboration or sharing semantics before promoting a finding.

## Build and execute the matrix

Read [access-matrix.md](references/access-matrix.md). For every applicable protected operation:

1. Capture the owner or authorized-role baseline.
2. Capture the unauthorized caller's own-object or allowed-function control when available.
3. Replay the same operation as unauthenticated, peer, cross-role, and cross-tenant callers.
4. Mutate one target dimension across path, query, body, nested objects, arrays, headers, cookies, forms, multipart fields, JSON Patch, GraphQL variables, aliases, fragments, and bulk/composite IDs.
5. Test evidenced method, content-type, route, API-version, action-name, and GraphQL operation variants.
6. Check related read/list/audit endpoints after any safe mutation; status alone never proves the action succeeded or failed.

Process every discovered operation or give a specific safety/non-applicability reason. Ranking controls order, not omission.

## Apply the five proof gates

Read [access-proof-gates.md](references/access-proof-gates.md). Accept a finding only when caller, target, ownership/tenant, expected denial, and unauthorized impact are all proven.

Treat these as leads only:

- HTTP 200, non-empty body, body-size/hash difference, matrix anomaly, or route name;
- empty/null/false responses, generic shells, validation errors, or 4xx/5xx reachability;
- current-user endpoints returning only the caller's data;
- guessed or shared object IDs without owner attribution;
- state-changing responses without authoritative readback.

## Reproduce independently

For accepted read findings, repeat the unauthorized request and an authorized control with fresh identity state. For mutations, create a fresh disposable object, prove its owner, capture before state, replay as the unauthorized caller, read back as the owner, restore or clean up, and record every step.

Have a separate reasoning pass challenge the expected-denial rule and evidence. It may reject or request follow-up; it must not invent stronger impact.

## Report coverage and results

Separate confirmed findings, candidates needing proof, expected behavior, safety-blocked tests, unusable identities, and untested operations. For each finding, provide exact reproducible requests with substitutable redacted values, actor/owner/tenant context, impact, cleanup status, and remediation at the authoritative enforcement layer.
