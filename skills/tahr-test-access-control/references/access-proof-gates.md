# Access-Control Proof Gates

Promote only when all five gates pass.

## 1. Caller proof

Identify the exact authenticated or unauthenticated caller, observed user, configured role, tenant, transport, and fresh validation state. A token claim or label alone is insufficient.

## 2. Target proof

Identify the exact resource, property, function, action, workflow transition, or GraphQL operation. Preserve method, path/document, identifier carrier, and object value using safe substitution or fingerprinting.

## 3. Ownership or tenant proof

Prove who owns or controls the target through an owner baseline, creation receipt, authoritative read, tenant-scoped list, documented relationship, or explicit assessment context. Guessed adjacent IDs and global ID pools are insufficient.

## 4. Expected-denial proof

Explain why this caller should be denied using application-specific evidence: policy code, middleware, documentation, owner/authorized-role baseline, role/tenant semantics, schema description, UI workflow, or explicit user context. Endpoint names and assumptions about admin or collaboration behavior are insufficient.

## 5. Unauthorized-impact proof

Show protected data returned, privileged function executed, forbidden property persisted, unauthorized state transition, cross-tenant resource exposed, or an equivalent concrete effect.

For mutations, require authoritative readback, disappearance, audit evidence, or another persistent effect. A success status or echoed body is not enough.

## Common rejection cases

- `/me`, preferences, subscription, organization, or profile endpoints return only caller-owned or empty state.
- The response is null, false, empty, a static shell, a validation error, or an unproven 500.
- Auth is stale, mismatched, browser-bound on the wrong transport, or otherwise untrusted.
- The target owner/tenant is unknown.
- The function appears privileged by name but no sensitive data or action is reached.
- A mutation response is not followed by readback.

Keep rejected rows as diagnostics with a reason. Preserve ambiguous rows as `needs_followup`; do not manufacture proof by inference.
