# Janus Protocol v4

External review edition, prepared 2026-09-20.

## Purpose

Janus is a protocol for using more than one AI model as one accountable system.

One session is the orchestrator. It owns the conversation, task state, risk classification, and final decision. A second model acts as a bounded peer with one named job. The peer returns evidence or criticism to the orchestrator and never owns the outcome.

Janus is designed to answer two questions:

1. When is a second model likely to improve the result enough to justify its cost?
2. How do you keep that second opinion independent, scoped, and auditable?

## The operating rule

Create the complete artifact first. Freeze it. Then spend one peer call on a specific review job.

Internal passes from the same model family can reduce omissions, but they are not independent review. The value of a peer comes from a different model family having a fair chance to find another causal frame, failure mode, or priority.

Keep permitted-action rules in a machine-readable policy file. Every prose rendering, including this document, is subordinate to that policy. Earlier Janus versions relied on documents that a model read and chose whether to follow. Rules were broken without detection, including by a model that had just read them.

## Roles

- **Orchestrator:** owns the user relationship, state, risk classification, final synthesis, and decision.
- **Explorer:** proposes alternative framings.
- **Builder:** constructs the strongest version of an option.
- **Adversary:** looks for concrete failure mechanisms and falsifiers.
- **Verifier:** checks load-bearing claims against primary evidence.
- **Auditor:** performs an optional second synthesis without inheriting the first model's conclusion.

Seats are functions, not brands. A model or tool should hold a seat only when its execution reliability and factual reliability meet the needs of that job.

## Dispatch tiers

| Tier | Use | Dispatch rule |
| --- | --- | --- |
| T1 | Bounded, reversible support with low blast radius | Silent dispatch is allowed |
| T2 | Substantive review that produces a durable artifact | Announce, then dispatch |
| T3 | Work with meaningful blast radius, authority transfer, material cost, or irreversible effects | Obtain operator approval first |

The following work is never silent: deployments, commits or pushes, deletion, external writes, schema migrations, repository-wide refactors, credential or billing work, legal or privacy work, security work, long-running research, shared infrastructure changes, and durable strategy.

## Data classification

Classification is separate from seat eligibility. A model may be qualified to do a job and still be forbidden from seeing the material.

| Class | Meaning | Hosted-peer rule |
| --- | --- | --- |
| `public` | Publishable material | May be dispatched |
| `internal` | Ordinary repository content, architecture notes, business decisions, and client strategy when disclosure to the provider is authorized | May be dispatched to an approved hosted peer |
| `restricted` | Material whose disclosure is limited by contract, privilege, regulation, or third-party rights | Do not dispatch to a hosted peer |
| `secret` | Credentials, tokens, passwords, keys, and vault contents | Never dispatch to a hosted peer; there is no override |

The classification is declared by the caller. Janus does not scan content and cannot detect a false label.

An adapter's data-trust level may impose a stricter limit. A low-trust adapter can be restricted to `public` material even when another approved peer may receive `internal` material.

## The brief

The brief is the whole job. It should contain:

- a request ID, tier, peer seat, expected output, allowed actions, and forbidden actions;
- the exact artifact under review;
- the question to attack, not the answer to confirm;
- at least one open question that gives the peer room to notice an unexpected issue;
- an explicit statement that finding no material problem is an acceptable result;
- a ban on reading raw capture logs or other files that could cause recursive ingestion.

When the peer helped create the artifact, instruct it to verify the result without taking credit for earlier influence.

## Cycle lifecycle

1. **Prepare:** finish the artifact and write the bounded brief.
2. **Classify:** assign the tier and data classification.
3. **Freeze:** hash the brief, policy, and artifact so later edits are visible.
4. **Approve:** for T3, bind approval to the exact brief, artifact, seat, peer, and expiration.
5. **Dispatch:** send the bounded package through the recorded runner.
6. **Capture:** preserve the complete response and execution metadata.
7. **Verify:** confirm the response belongs to this cycle and that the stored hashes still match.
8. **Judge:** decide whether the response is substantively responsive and correct enough to use.
9. **Accept:** record whether the peer changed the recommendation, ranking, risk posture, next action, or eliminated an option.

An official Janus cycle exists only when the runner creates and records it. An unrecorded model consultation may still be useful, but it is not a Janus cycle.

## What a runner must enforce

These are requirements for any implementation. The current Janus runner enforces them as refusals.

- no peer dispatch before a complete artifact exists;
- one scarce peer call per cycle unless an explicit retry is granted;
- no nested peer dispatch;
- seat eligibility and trust floors;
- data-classification limits;
- T3 approval bound to the exact review subject;
- frozen brief and artifact hashes;
- full, untruncated capture outside the peer's read path;
- state changes only while holding the cycle lease;
- a receipt that binds the response to the cycle;
- explicit handling for timeouts, empty output, nonzero exits, wrong-brief responses, and stale cycles;
- an outcome record after human synthesis.

## Receipts and their limits

A receipt should record what was requested, the peer and model arguments sent, the exact payload hash, the response hash, exit status, and the cycle token.

A receipt does not prove the answer is good. A correlation token can show that the response came from the current brief, but it cannot show that the peer understood the question or answered it well. Substantive acceptance remains a human judgment.

Requested model and effort are provenance about the invocation. They do not prove that a provider honored those settings internally.

## Failure behavior

Silence is not success.

- A nonzero exit, empty capture, partial capture, or missing cycle token fails the cycle.
- A response to the wrong brief fails and may receive at most one explicit redispatch.
- A timeout or quota exhaustion during a stage marks the cycle stale. The operator must decide whether to resume and spend another call.
- A crash leaves visible, nonterminal state. It must never trigger an automatic replay.
- A late response after cancellation is discarded as stale.

## Measuring whether Janus is worth using

The peer earns its place only when it changes what the orchestrator does.

For every completed cycle, record whether the peer:

- changed the recommendation;
- changed the ranking of options;
- changed the risk posture;
- eliminated an option;
- changed the next action.

Review a rolling cohort after six judged cycles. If fewer than two changed any of those outcomes, tighten the trigger. For example, require a named unresolved risk or a consequential decision before authorizing another peer call.

## Security boundary

Janus is an accounting and discipline boundary. It is not a security or compliance control.

It does not prevent a person or agent from contacting a model outside the runner. It cannot detect all bypasses. It does not hold credentials outside the model's reach. It does not make prompt-injected artifacts safe. It does not certify that a peer answer is correct.

A team or compliance deployment needs a stronger brokered design: credentials outside the model-facing shell, explicit process and network capabilities, operator approval through a separate trusted channel, and receipts issued by the broker.

## Adoption checklist

Before using Janus in a new environment:

1. Put the governing rules in a machine-readable policy file and define what wins when prose disagrees.
2. Name the orchestrator and peer model families.
3. Define the available seats and trust floors.
4. Record evidence-based strengths, weaknesses, and recheck triggers for each adapter.
5. Define the four data classes and who may receive each one.
6. Decide what counts as a scarce invocation.
7. Implement frozen briefs, artifact hashes, full response capture, and receipts.
8. Define T3 approval outside ordinary model self-report.
9. Test refusal paths, concurrency, crashes, timeouts, wrong-brief responses, and tampering.
10. Record outcome changes and review calibration after six judged cycles.

## Honest status

The current Janus implementation is a working local tool for trusted operators who read and judge every response. It has been repeatedly adversarially reviewed and tested, but green checks do not prove the implementation complete. It is not presented as a finished security product.
