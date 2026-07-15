# Access-Control Matrix

## Subject relationships

Test applicable rows independently:

| Caller | Target relationship | Question |
|---|---|---|
| Unauthenticated | Protected resource/function | Is authentication enforced at the authoritative backend? |
| Same user | Own object | Does the valid control establish the operation and response semantics? |
| Same-role peer | Another user's object | Is ownership enforced independently of role? |
| Lower privilege | Privileged function | Is function authorization enforced on every method and route? |
| Cross-tenant peer | Other tenant's object | Is tenant scoping derived server-side? |
| Higher privilege | Lower user's private object | Does admin scope match documented policy rather than assumed omniscience? |
| Service/integration actor | User/admin function | Are token audience, scope, and actor type enforced? |

## Identifier and property carriers

Cover each carrier present in real requests:

- path and query identifiers;
- JSON/form body fields, nested objects, arrays, and bulk/composite IDs;
- headers and cookies carrying owner, tenant, role, account, or object context;
- multipart metadata and filenames;
- JSON Patch paths/values and DELETE bodies;
- GraphQL variables, input objects, aliases, fragments, nested selection paths, mutations, and subscriptions.

High-value fields include `id`, `user_id`, `owner_id`, `account_id`, `tenant_id`, `organization_id`, `workspace_id`, `role`, `permissions`, `status`, `approved`, `verified`, `price`, `credit`, and `entitlement` when the target actually uses them.

## Variant matrix

Test evidenced variants, not generic path spraying:

- GET/POST/PUT/PATCH/DELETE and supported override mechanisms;
- JSON, form, multipart, JSON Patch, XML, or other advertised parsers;
- trailing/case/normalization variants handled by distinct routing layers;
- current, mobile, legacy, beta, or alternate API versions;
- singular, bulk, export, import, and asynchronous equivalents;
- direct URL access, reordered workflow step, stale state, repeat action, alternate actor, and cross-tenant continuation.

## Matrix cell record

Retain caller identity and trust status, owner identity, tenant relationship, exact request shape, object provenance, expected rule and source, response class, readback/control references, and terminal status. Matrix anomalies are diagnostic until the proof gates pass.
