# High-Risk Change Checklist

Use this checklist only when `RULES.md` classifies a change as high-risk. Apply relevant sections rather than mechanically performing unrelated checks.

## Classification

Treat a change as high-risk when a defect could be security-relevant, irreversible, or reach production or shared state before detection, including:

- authentication, credentials, secrets, authorization, sandboxing, and path, URL, Git-ref, or YAML guards;
- CI/CD, release workflows, GitOps, DNS/TLS, image publication, credential scope, and branch protection;
- migrations, deletion, retention, encryption formats, locks, idempotency, and external side effects;
- concurrency, retries, timeouts, cancellation, and background workers on production paths;
- cross-service APIs, events, protocols, serialization, behavior-changing defaults, and shared libraries;
- rewriting or force-pushing shared history.

Docs-only, isolated UI-copy, and local-tooling changes are low-risk by default. When uncertain, treat changes that are not quickly and fully reversible as high-risk.

## Review the complete change

- Review the exact final diff and generated merge result, every changed file, affected callers and dependencies, runtime paths, deployment paths, and rollback behavior.
- Check compatibility, resource exhaustion, performance, failure timing around external side effects, and behavior that relied on an earlier decision.
- Compare documented limits, timeouts, defaults, flags, and schemas with the implementation.
- Inspect disabled and alternate paths where relevant: feature off, background or cron execution, synchronous invocation, retry, and cancellation during each external step.
- Validate integrations against the counterpart's pinned implementation and the way it parses errors, responses, and types.

## Test failure modes

- Inject failure and cancellation at new connections, locks, slots, time budgets, persistence boundaries, and “persist then act” transitions.
- Verify ordinary errors do not corrupt process-global state and that safety tests can still fail in the full test session.
- Treat “behavior-preserving” refactors as claims to verify against the last reviewed version.
- For concurrency-sensitive changes, use observable barriers and repeat tests enough to expose nondeterminism.
- Test real counterparts only in a safe non-production environment unless the user explicitly authorizes production interaction. Add proxy latency, compression, and concurrent calls only when those behaviors are part of the supported contract.

## Validate delivery when applicable

- Trace artifact publication, configuration promotion, and runtime reconciliation through the exact revisions and serialized values that will be deployed.
- Render every supported chart or source mode, compare applicable parity anchors, and verify certificate names, annotation types, registry credentials, persistent-volume options, and base/environment merge order.
- Preserve immutable legacy fixtures and explicitly validate retained deployment targets, case-sensitive policy values, staged activation, and controller behavior that can outlive removed declarations.
- Confirm credential restrictions and branch protections on the server; distinguish a prepared migration from externally verified enforcement.
- Gate cross-service phases on observed health rather than assuming independent deployment resources execute in source order.
- Compare the live or final pod template with the intended baseline, including probes, before calling a staging rollout complete.

## Final evidence

- Run one fresh, adversarial review of the final `base..head` state. Add a second specialized security, fault-injection, or deployment review only when that independent risk justifies the cost.
- Record the exact commit or merge result reviewed, tests performed, artifact identity, downstream verification, unresolved limitations, and evidence for every retained-by-design finding.
- If verification requires unavailable authority, unsafe production interaction, or an inaccessible counterpart, report the gap instead of claiming completion.
