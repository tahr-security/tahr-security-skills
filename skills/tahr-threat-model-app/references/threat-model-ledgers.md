# Threat Model Ledgers

Use stable IDs and evidence references. Keep threat candidates, supporting
evidence/proof, validation results, and coverage accounting distinct. A threat
catalog entry or missing-control hypothesis is a candidate until it connects to
an implementation-backed asset, flow, boundary, and abuse case.

## Entity inventory

Record actors, assets, components, trust zones, controls, data stores,
integrations, and threats.

```json
{
  "entity_id": "ASSET-001",
  "type": "actor|asset|component|trust_zone|control|data_store|integration|threat",
  "name": "Tenant export archive",
  "description": "",
  "data_classification": "sensitive",
  "evidence_class": "observed|intended|inferred|unknown",
  "evidence_refs": [{"source": "src/export/service.ts", "location": "buildExport"}],
  "confidence": "high|medium|low",
  "open_questions": []
}
```

## Data-flow record

```json
{
  "flow_id": "FLOW-001",
  "name": "User requests tenant export",
  "actor_ids": ["ACTOR-USER"],
  "input": "tenant and export parameters",
  "entrypoint": "POST /api/exports",
  "component_path": ["API", "export service", "worker"],
  "asset_ids": ["ASSET-001"],
  "boundary_crossings": ["browser-to-api", "api-to-worker"],
  "controls_observed": [],
  "controls_intended": [],
  "controls_assumed": [],
  "sink_or_final_state": "object storage archive",
  "evidence_refs": [],
  "unresolved_questions": []
}
```

Include asynchronous workers, callbacks, redirects, external services, and
response/render paths. Do not compress a material boundary crossing into a
generic “backend” node.

## Security decision and invariant

```json
{
  "decision_id": "DEC-001",
  "invariant": "Tenant scope must be enforced before export data is selected.",
  "owner": "unknown",
  "related_flows": ["FLOW-001"],
  "observed_control": null,
  "intended_control": "tenant-scoped repository query",
  "status": "implemented|partial|assumed|missing|unknown",
  "evidence_refs": [],
  "validation_test_ids": ["TEST-001"]
}
```

## Threat and abuse-case record

```json
{
  "threat_id": "THREAT-001",
  "title": "Cross-tenant data included in export",
  "actor": "authenticated tenant user",
  "goal": "obtain another tenant's records",
  "asset_ids": ["ASSET-001"],
  "flow_ids": ["FLOW-001"],
  "preconditions": [],
  "boundary": "tenant authorization boundary",
  "abuse_path": [],
  "existing_controls": [],
  "missing_or_assumed_controls": [],
  "contradiction_checked": [],
  "business_impact": "",
  "risk": "critical|high|medium|low",
  "confidence": "high|medium|low",
  "evidence_refs": [],
  "status": "modeled|assumption|validation_required|rejected"
}
```

Keep risk and confidence separate. A high-impact scenario with weak evidence
may remain high risk but low confidence and must be labeled validation required.

## Control matrix

Create one row for each material asset/flow/threat combination:

```json
{
  "matrix_id": "CTRL-001",
  "asset_id": "ASSET-001",
  "flow_id": "FLOW-001",
  "threat_id": "THREAT-001",
  "visible_controls": [],
  "missing_controls": [],
  "assumed_controls": [],
  "control_owner": "unknown",
  "framework_mappings": ["STRIDE:E", "ASVS:authorization"],
  "validation_test_ids": ["TEST-001"],
  "confidence": "medium"
}
```

## Validation test case

```json
{
  "test_id": "TEST-001",
  "threat_id": "THREAT-001",
  "actor": "tenant A user",
  "asset": "tenant B export data",
  "boundary": "tenant authorization",
  "preconditions": ["two disposable tenants", "owner-attributed records"],
  "setup": [],
  "objective": "Verify cross-tenant identifiers cannot influence export selection.",
  "expected_control": "server-derived tenant scope",
  "positive_signal": "tenant A receives tenant B data",
  "negative_signal": "request is denied or result remains tenant A scoped",
  "evidence_to_collect": [],
  "safe_environment": "local|staging",
  "confidence": "medium",
  "evidence_refs": []
}
```

## Coverage ledger

Account for each high-signal entrypoint, auth/authz file, asset, boundary, flow,
integration, worker, admin surface, deployment zone, and decision.

Use `reviewed`, `reviewed_no_issue`, `out_of_scope`, or
`deferred_with_specific_reason`. Any high-risk `pending`, generic deferred
reason, or unread item forces `incomplete_high_risk_coverage`.
