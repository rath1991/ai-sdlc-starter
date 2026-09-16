---
name: tool-design
description: The standard for designing a tool — contract, idempotency, side effects, error taxonomy and authorization. Use for any deterministic capability, including components demoted from agents.
stage: blueprint
used_by: [technical-blueprint, reusable-qualification, recipe-compiler]
---

# Tool Design Standard

Tools are the deterministic layer. **Prefer them.**

Every capability implemented as a tool rather than an agent is one that can be unit
tested, reasoned about, cached, and debugged from a stack trace instead of a transcript.
Components demoted here by [`agent-design`](../agent-design/SKILL.md) are a success, not a
downgrade.

## Required specification

### Contract

```yaml
name: document_retrieval
description: Retrieve documents matching a query within a scope    # the agent reads this
parameters:
  - name: query
    type: string
    required: true
  - name: scope
    type: object
    required: true
    schema: recipes/schemas/retrieval_scope.json
  - name: limit
    type: integer
    default: 20
    max: 100
returns:
  type: object
  schema: recipes/schemas/retrieval_result.json
```

The `description` is part of the contract, not documentation: it is what the calling agent
uses to decide whether this tool applies. A vague description produces misuse that looks
like a model failure.

Constrain parameters at the contract. A `limit` with a maximum cannot be used to request a
million rows. Validation belongs in the contract, where it is enforced once, rather than in
the caller, where it is enforced sometimes.

### Idempotency

State it explicitly: **idempotent** or **not**.

If not, say what repeat invocation does and whether the caller must deduplicate. Agents
retry. A non-idempotent tool that is retried is how one action becomes three, and the agent
will not know it happened.

### Side effects

| | |
|---|---|
| **Reads** | Which systems, which data |
| **Writes** | Which systems, what mutation |
| **External calls** | What leaves the boundary |
| **Cost** | Per invocation, where it is non-trivial |

Separate readers from writers. A tool that both queries and mutates cannot be safely
retried and cannot be safely explored by an agent. If you need both, make two tools.

### Error taxonomy

Typed errors, not strings. The agent has to be able to distinguish "you asked wrongly" from
"the system is down," because the correct response differs.

| Error | Meaning | Retryable | Agent should |
|---|---|---|---|
| `INVALID_PARAMETER` | Caller error | no | Fix the call |
| `NOT_FOUND` | Nothing matched | no | Proceed with empty |
| `UNAUTHORIZED` | Missing permission | no | Surface — do not retry |
| `UPSTREAM_UNAVAILABLE` | Dependency down | yes, with backoff | Retry, then surface |
| `TIMEOUT` | Exceeded budget | yes, once | Retry, then partial or surface |
| `QUOTA_EXCEEDED` | Rate limited | yes, after delay | Back off |

**Never return a plausible empty result in place of an error.** An agent receiving `[]`
from a failed retrieval concludes there is no evidence and proceeds confidently. This is
the single most damaging tool-design defect, because it produces a wrong answer with no
error anywhere in the trace.

### Authorization

- What permission is required?
- Whose identity is used — the user's, or a service identity?
- What must the tool refuse even when technically able?
- Is the call audited?

A tool running under a service identity with broad access, invoked on behalf of a user with
narrow access, is a privilege escalation with extra steps. State the identity model.

### Determinism and caching

- Same input, same output? If not, why?
- Cacheable? Key and TTL.
- Does it depend on wall-clock time or external mutable state?

## Review checklist

- [ ] Description is precise enough for correct tool selection
- [ ] Parameters typed, constrained, validated at the contract
- [ ] Return schema defined
- [ ] Idempotency stated
- [ ] Reads and writes separated
- [ ] Side effects enumerated
- [ ] Errors typed, with retryability
- [ ] **Never returns empty in place of an error**
- [ ] Identity model stated
- [ ] Refusals stated
- [ ] Traces to a Spec ID
