# Threat Evidence And Quality Gates

## Evidence classification

- `observed`: directly supported by current source, configuration, or IaC.
- `intended`: stated in a specification, diagram, role matrix, requirement, or
  control document.
- `inferred`: reasoned from multiple observations but not directly implemented
  or documented.
- `unknown`: important fact not established by available evidence.

Source is authoritative for observed implementation. Documents explain intent
and business importance. Runtime evidence, when safely authorized and captured,
can confirm deployed behavior. Never silently promote intended or inferred
control into observed control.

## Contradiction gate

Before modeling a control as missing or bypassable:

1. Trace the complete component and data-flow path.
2. Search global and local middleware, service/domain policies, repository
   scopes, serializers, validators, framework hooks, and deployment controls.
3. Distinguish control declaration from invocation and invocation from
   effective enforcement.
4. Record the exact evidence that supports or contradicts the gap.
5. Use `unknown` plus a validation test when applicability remains ambiguous.

## Material threat gate

Retain a threat only when it answers:

- Which actor starts the abuse case?
- Which asset or security property is at stake?
- Which flow and trust boundary enable it?
- Which preconditions and attacker goal apply?
- Which observed or intended control limits it?
- What business impact follows?
- What decision or test should happen next?

Omit or demote a generic code smell that cannot connect to an asset, boundary,
flow, abuse case, attacker path, or security decision.

## Attack-path gate

Build an attack path only from connected model elements. Preserve actor,
preconditions, entrypoint, boundary crossings, required threats/control gaps,
intermediate assets, and final impact. Do not invent a step to make two risks
form a chain. Keep uncertain steps conditional.

## Risk-ranking gate

Rank with:

- exposure and required privilege;
- attacker complexity and user interaction;
- asset sensitivity and tenant/role reach;
- confidentiality, integrity, availability, privacy, financial, and operational
  impact;
- visible and compensating controls;
- source/spec/document coverage confidence;
- business-workflow criticality.

Keep confidence separate from impact. Do not label modeled risk as verified
vulnerability or confirmed exploitation.

## Privacy applicability gate

Enable privacy analysis only when source or documents identify personal,
sensitive, behavioral, location, identity, or regulated data. Examine
linkability, identifiability, disclosure, unawareness/consent, retention, and
compliance boundaries. Otherwise record `applicable: false` and the evidence
used to decide.

## AI applicability gate

Enable AI analysis only when the application uses models, prompts, RAG,
embeddings, agents, tools, MCP, evaluation gates, or model providers. Examine:

- direct and indirect prompt injection;
- retrieval and tool authorization tied to the initiating identity/tenant;
- data exfiltration across context, memory, logs, or providers;
- unsafe tool side effects and confused deputy behavior;
- model, prompt, plugin, and supply-chain trust;
- output trust, evaluation bypass, and wallet/quota abuse.

Do not treat model use alone as a vulnerability.

## Validation-test safety gate

Specify a test without executing it unless the user authorizes the exact local
or staging target. Require disposable data, bounded non-destructive actions,
normal baseline, expected denial/non-execution negative control, success and
failure signals, and minimal evidence collection. Never include raw secrets or
customer data.

## Final quality gate

Reject a “complete” model when:

- critical assets, trust boundaries, or principal actors are absent;
- output is route-by-route without system flows;
- claims lack evidence refs or assumption labels;
- attack paths lack connected elements or business impact;
- recommendations are generic rather than decisions/tests;
- available diagrams, specs, roles, or deployment notes did not affect the
  model;
- high-risk files, flows, integrations, admin surfaces, or controls remain
  unread or unresolved;
- validation cases omit actor, asset, boundary, preconditions, expected
  control, or negative signal.

Use `incomplete_high_risk_coverage` until every material gap is dispositioned.
