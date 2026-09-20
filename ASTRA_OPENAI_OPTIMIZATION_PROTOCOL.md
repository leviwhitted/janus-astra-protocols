# Astra/OpenAI Model Optimization Protocol

External review edition, prepared 2026-09-20.

## Purpose

This protocol chooses the least expensive model route that can reliably produce an accepted result. Cost includes retries, review, human intervention, elapsed time, and orchestration overhead. Token price alone is not the objective.

The routing table below is a starting hypothesis. It is not a measured leaderboard. Each team should calibrate it on representative work.

## Evidence boundary

Official OpenAI documentation establishes current model names, supported reasoning settings, and general product positioning. It does not establish which route will perform best on a specific team's work.

OpenAI's published model-selection guidance recommends reaching the accuracy target first, then optimizing cost and latency. This protocol follows that order. See the [OpenAI model-selection guide](https://developers.openai.com/api/docs/guides/model-selection).

Model availability can differ across the API, ChatGPT, and Codex. Verify the active product surface before assigning work. OpenAI documents these product-surface distinctions in its [workspace model availability guidance](https://learn.chatgpt.com/docs/enterprise/workspace-model-availability).

## Starting routes

| Work | Starting route | Return to Astra when |
| --- | --- | --- |
| Rename, reformat, extract fields, or assemble known inputs | GPT-5.6 Luna | Requirements conflict or source meaning is ambiguous |
| Bounded research, factual summaries, or variants from an approved brief | GPT-5.6 Terra | Sources disagree, research changes strategy, or claims require consequential judgment |
| Coding, debugging, technical audits, approved design implementation, or browser QA | GPT-5.6 Sol | A focused repair fails, architecture must change, or competing constraints require judgment |
| Difficult synthesis, cross-project priorities, consequential creative direction, or final high-stakes judgment | GPT-6 Astra | This is the Astra stage; hand bounded execution back once the direction is clear |
| Image or media creation | A dedicated generation or editing tool under an approved brief | Brand or product tradeoffs require a higher-level decision |

OpenAI currently describes GPT-6 Astra as its most capable model, GPT-5.6 Sol as a flagship model for complex professional work, GPT-5.6 Terra as the intelligence-and-cost balance, and GPT-5.6 Luna as the cost-sensitive high-volume option. Those descriptions support the starting routes, but they do not validate task-specific superiority. See the [OpenAI model catalog](https://developers.openai.com/api/docs/models) and [GPT-6 Astra guidance](https://developers.openai.com/api/docs/guides/latest-model).

## Choose effort separately

Model and reasoning effort are separate decisions. Do not treat higher effort as a status signal.

- Start near the middle of the available effort range for consequential interactive work when no local evidence supports another choice. For the currently named models, that starting point is `medium`.
- Use lower effort for bounded, mechanical work with objective checks.
- Increase effort when the task has difficult ambiguity, long dependency chains, or consequential cross-system tradeoffs.
- Record the selected model, effort, and product surface. A model cannot reliably infer its own effective reasoning setting.
- Explain a proposed model or effort change before switching. The operator makes the change.

Official documentation says GPT-6 Astra supports `low`, `medium`, `high`, `xhigh`, and `max` reasoning effort. It does not support `none`. The GPT-5.6 family also supports `none`. Recheck the [current model catalog](https://developers.openai.com/api/docs/models) before treating these values as stable.

## Work loop

1. **Define one outcome.** Name the project, exact artifacts, constraints, and observable acceptance checks.
2. **Separate judgment from execution.** Use Astra when choosing direction is the hard part. Use a sufficient lower-cost route for a bounded implementation with clear checks.
3. **Prepare the handoff.** Include source paths, approved facts, prohibited actions, expected output, and stopping conditions. Do not transfer an entire conversation when a smaller evidence packet will do.
4. **Execute one useful batch.** Return the artifact, changed files, checks, material uncertainty, and decisions still needed.
5. **Allow one focused repair.** When a clear defect remains, repair it once. If the same failure survives, reassess the brief, evidence, tool access, or route.
6. **Accept against evidence.** Objective checks can close low-risk work. Consequential tradeoffs return to the orchestrator or operator.
7. **Record the outcome.** Capture enough evidence to improve the next routing decision, then stop.

## Handoff template

Use this structure for substantial work:

```text
Outcome
[One concrete result to produce]

Inputs
- [Exact source paths, links, or attached artifacts]

Allowed actions
- [Read, edit, run tests, use browser, create local artifact]

Forbidden actions
- [Deploy, publish, send messages, change accounts, spend money, delete data]

Constraints
- [Facts, copy, interfaces, policies, and boundaries to preserve]

Acceptance checks
- [Observable conditions that establish success]

Return
- Artifact locations
- Changed files
- Checks run and results
- Material uncertainties
- Decisions still needed
```

## Review and peer challenge

The user-facing orchestrator owns the final synthesis. A second model may supply objections, evidence checks, or alternative framings, but severity of tone is not evidence.

For each material peer finding, record one disposition:

- **Adopt:** the evidence supports the finding as stated.
- **Modify:** the core issue is real, but the scope or remedy changes.
- **Reject:** the evidence does not support the finding.
- **Unresolved:** available evidence cannot settle it.

Use the Janus protocol when an independent challenge is likely to change a consequential decision. Do not request review only because a prior reviewer found something.

## Evaluation record

For each meaningful task, capture:

| Field | Record |
| --- | --- |
| Task | The bounded outcome and task class |
| Route | Exact model, effort, and product surface |
| Inputs | Source artifacts and relevant versions |
| Acceptance | Checks and human verdict |
| Repairs | Number and type of focused retries |
| Intervention | User or orchestrator effort required |
| Time and usage | Observed values only; do not infer task usage from an account-wide quota change |
| Unknowns | Missing evidence or product behavior not established |

One success establishes feasibility for that task. Repeated acceptable results across comparable tasks may justify a narrow default. Persistent failures justify revisiting the route.

Run a paired comparison only when its result could change routing enough to repay the experiment's cost. Freeze the inputs, rubric, rejection conditions, and scoring process before viewing candidate outputs.

## Operating boundaries

- Do not sacrifice required quality or factual accuracy to save tokens.
- Do not send broad discovery to Astra by default.
- Do not keep Astra polling a worker or rewriting competent output for style alone.
- Do not silently fall back to another model or effort.
- Do not claim model superiority from one favorable run.
- Do not confuse API pricing with subscription usage or product access.
- Do not let a model review its own earlier conclusion as the only independent evaluator.
- Do not deploy, publish, contact people, change accounts, install integrations, or spend money without explicit authority.

## Calibration questions

Review these after a useful sample of completed tasks:

1. Which routes consistently meet acceptance without repair?
2. Which task types consume the most human intervention?
3. Where does a lower-cost route preserve quality?
4. Where does Astra materially change the decision?
5. Which handoff fields prevent rework?
6. Which failures come from model choice, and which come from a weak brief or missing evidence?

The protocol is working when routing becomes narrower and more evidence-based over time, not when every task uses the most capable model.
