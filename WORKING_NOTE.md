# Kaggriculture Agent Working Note: Dynamic Economic Simulation

**Language:** [English](WORKING_NOTE.md) | [简体中文](WORKING_NOTE.zh-CN.md)
**Scope:** Kaggle Kaggriculture two-player farming competition
**Archived agent:** Kaggle submission `56720623`, [`main.py`](main.py)
**Score snapshot:** 2026-10-19
**Document type:** Working note

## Abstract

This note records the development and validation of one agent for a 720-step, two-player farming and market simulation. At each step, an agent schedules farmers and workers, crops, livestock, inventory transfers, and ordered market transactions. The central difficulty is not selecting a single high-price crop. It is coordinating capital, production lead time, labor actions, maintenance inputs, storage, and a shared price process while the opponent and shop mix vary.

The final public artifact is one complete Python agent. It preserves the existing production policy and adds a narrowly gated purchase-order timing layer. When the visible farm layout is sufficiently similar, cash is adequate, and the simulated advantage clears a threshold, the layer may reorder already-selected wheat or fertilizer purchases relative to a fixed-price seed order. This targets a local market-execution bottleneck; it is not a replacement production planner.

The recorded Kaggle result for submission `56720623` is public score `1662.0` and private score `4279.6`, with status `COMPLETE`. These values are platform observations for one competition snapshot. They are not a general estimate against arbitrary opponents and do not isolate the causal effect of any single code change.

## 1. Task and Environment

Kaggriculture is a turn-based, multi-agent farm-management simulation. A default season has 30 days with 24 steps per day, for 720 steps. Each step supplies an observation and accepts actions for the farmer and hired workers, together with ordered market orders. A farm starts with limited land and cash. It can purchase land, seeds, animals, inputs, and labor, then earn cash through planting, feeding, watering, harvesting, hauling, and selling.

The task combines strategy and systems engineering:

- **Finite-horizon economics:** an investment must recover within the remaining season; early cash preservation competes with expansion.
- **Spatial and action budgets:** land, worker count, movement, storage, and per-step order limits constrain realized capacity.
- **Shared dynamic market:** both players change market inventory and prices; shops create continuing demand for randomly selected products.
- **Partial observability:** farm and market fields are visible in different degrees, while opponent inventory is not fully observable.
- **Stochastic evaluation:** shop configurations and opponents vary, so one seed or one final-cash difference is not a complete strength estimate.

The active rules and engine version are authoritative. Older notebooks can become stale when action order, lifecycle details, pricing, or evaluation changes.

## 2. Research Questions

The work focused on three questions:

1. Which economic and execution rules determine whether a production plan is feasible rather than merely profitable on paper?
2. When production assets are already similar, can market-order timing remove a repeatable execution loss without changing the plan?
3. How can a candidate be separated into strategy evidence, replay evidence, and packaging evidence before submission?

## 3. Method

### 3.1 Turn the environment into a checkable model

The first pass checked agent input and output, step ordering, crop growth and decay, animal maintenance, worker cost, storage capacity, market order limits, price updates, and shop demand. Details with direct strategic impact included:

- A newly planted crop contributes to the same day's water deficit count; missed maintenance can erase the investment quickly.
- Animals require continuing wheat input; maintenance failure can remove the animal rather than merely reduce one harvest.
- Marginal worker cost increases with the number hired that day; more workers do not automatically mean more output.
- Non-seed inventory is storage-limited; end-of-step overflow can be lost, while hauling and selling consume action or order budget.
- Orders in one step are ordered and processed in alternating player positions; the same purchase can see a different quote at a different position.
- Additional shops change persistent demand, and repeated shop types are possible.

These rules make a static price table insufficient. A useful model must include growth delay, feed cost, movement, shared-market impact, and overflow risk.

### 3.2 Use trajectories as event ledgers

For complete official-environment games, the analysis compared cash, land, workers, crops, livestock, and realized transactions at fixed checkpoints. Near-mirror games were replayed step by step to verify sale timing, quantity, price, and wheat purchase cost rather than inferring causes from the final score alone.

A broader replay sample covered 230 complete games across 29 rank bands. A separate recorded online baseline contained 98 complete games: 42 wins, 22 draws, and 34 losses. The purchase-timing candidate was counterfactually replayed on the baseline's wins and losses. These samples contain matching, opponent-pool, and version bias; their counts are diagnostic evidence, not random-sample win-rate estimates.

### 3.3 Separate three kinds of gap

The same pattern appeared repeatedly:

1. **Structural gap:** lagging land, production mix, or early expansion cannot usually be repaired by changing sale order alone.
2. **Execution gap:** with similar production assets, purchase timing, order queue position, product mix, and final-day liquidation can change a close result.
3. **Runtime gap:** source parsing, schema compliance, serialization, dependencies, and full-episode completion require independent acceptance checks.

Keeping these gaps separate makes conclusions more reproducible than changing production, market, and packaging simultaneously against one total score.

## 4. Final Agent

The archived [`main.py`](main.py) is byte-identical to the source used for Kaggle submission `56720623`. It is an integration of public strategy components, historical layers, and a new outer layer; it should not be described as a fully original strategy written from scratch.

The final callable is `purchase_timing_agent`. It starts with the parent strategy's actions and only considers swapping adjacent input and fixed-price seed orders when all relevant guards pass:

- visible farm-layout similarity is at least `0.90`;
- only wheat or fertilizer purchases already present in the parent output may be reordered;
- no new product, quantity, production action, or investment decision is introduced;
- the ordered market is revalued under the public lock-step price mechanism;
- the predicted advantage exceeds 20 game coins and a cash buffer remains;
- on an exception or inconsistent state, the parent actions are returned unchanged.

The design is intentionally conservative. It does not read hidden opponent inventory, account identity, seed number, or episode ID, and it does not replay a fixed episode's action tape.

The source retains Apache-2.0 notices and upstream attribution in `main.py`. Preserve those notices when redistributing or modifying the file.

## 5. Evaluation Record

| Evidence | Observation | Supports | Does not establish |
| --- | --- | --- | --- |
| Kaggle submission `56720623` | `COMPLETE`; public `1662.0`, private `4279.6` | The platform accepted and completed this submission; these are the recorded scores | Generality across opponent pools or causality for one edit |
| Official full-episode smoke test | `kaggle-environments` 1.32.7; 720 steps; both seats `DONE` | The entry point loads and completes a full tested episode | Win rate against hidden opponents |
| Kaggle-style source parsing | No BOM; raw Python compilation succeeded; callable resolved as `purchase_timing_agent` | Basic packaging and entry-point correctness | All online environment differences |
| Counterfactual replay on the recorded baseline | 34 losses and 42 wins replayed; 3 losses flipped, other losses unchanged; no prior win became a loss; 1 win improved | No observed regression on this selected trace set | Independent randomized evaluation |
| Official two-seat comparisons | 24 mirror games over 12 unused seeds were draws; most comparisons against other tested strategies were positive | Compatibility on near-mirror and selected panels | Broad robustness; some production structures still produced large negative margins |

The underlying per-game logs and temporary replay material remain in the private work area and are not part of this repository. The table reports only the evidence needed to interpret the archived version.

## 6. Lessons for Similar Problems

### Plan complete-cycle economics

Compare full-cycle net return rather than unit price. Include initial purchases and land, time to first output, maintenance inputs, worker actions, harvest and hauling capacity, storage, and price feedback from expanding the same product. Evaluate capital spending against a budget, cash buffer, and conservative demand scenarios.

### Treat capacity and liquidation as different stages

Early investment may not become saleable output before the middle of the season. Conversely, inventory without workers, hauling, or sale-order budget cannot be realized. Track cash, assets, and saleable inventory by time segment so a gap can be assigned to capital allocation, production, logistics, or price execution.

### Do not overfit to one shop configuration

Shops alter marginal demand, but a single observed shop should not hard-code a crop or livestock count. Future shops are uncertain, the market is shared, and repeated shops can occur. Test unseen combinations and use shop composition as a scenario variable rather than a deterministic recipe.

### Treat small execution gains as local until proven otherwise

A measurable purchase-queue gain did not repair structural differences caused by production mix, extra land, late tomato output, or sales structure. Several fixed expansion experiments regressed in some worlds and were excluded. The supported claim is a bounded incremental improvement under the tested evidence, not a new production planner.

### Use scores as signals, not diagnoses

Online ratings depend on the matching pool, opponent strategies, and schedule. Full replays can explain why a game was lost, but downloading only losses creates selection bias. Include wins, draws, independent opponents, both seats, different shop mixes, and unused seeds. Near-mirror self-play is useful for execution differences, not sufficient for cross-strategy strength.

### Validate engineering separately

Before submission, use the parser and entry-point behavior that the platform actually uses. Check the first bytes and encoding, resolved callable, action schema, dependency availability, serialization, and a complete 720-step run. Local syntax compilation alone can miss a packaging failure such as a UTF-8 BOM.

## 7. Reusable Workflow

1. **Read the contract.** Record the current rules, execution order, observation schema, action schema, terminal condition, and scoring method.
2. **Build a transparent baseline.** Make actions legal and runs stable before optimizing; retain a rollback version and source hash for every iteration.
3. **Convert replays into event ledgers.** Record production, maintenance, purchases, sales, wages, storage, and terminal cash. Reproduce events in the official engine where possible.
4. **State falsifiable hypotheses.** Change one mechanism at a time; define the expected movement, side effects, and rejection conditions. Separate tuning traces from unseen seeds and independent opponents.
5. **Select by risk report.** Report sample size, win/draw/loss counts, means, worst cases, and strata by world features. Make any mean-versus-tail-risk trade-off explicit.
6. **Accept the complete agent before submission.** Check parsing, dependencies, BOM, serialization, both seats, and a full episode.
7. **Publish conclusions with failure conditions.** Label observations versus inferences, identify versions and data sources, and avoid releasing credentials or private logs.

This workflow also applies to market simulators, resource schedulers, management games, and other long-horizon multi-agent benchmarks.

## 8. Reproduction and Sources

- Competition: [Kaggle Kaggriculture](https://www.kaggle.com/competitions/kaggriculture)
- Public strategy sources retained in the code notices: [Thomas Tschinkel public state-router](https://www.kaggle.com/code/thomastschinkel/kaggriculture-93-8-win-rate-public-state-router?scriptVersionId=347936183) and [Yusuke Hayashi Shop Router](https://www.kaggle.com/code/yhay81/shop-router-0913)
- Local engine used for validation: `kaggle-environments==1.32.7`
- Kaggle submission: `56720623`
- Archived source SHA-256: `4bb6df2f8bb0e15c1c8e6666dca9102d47c6180c7f6c9407f6b1dd979a1ea5d3`

The repository contains the final agent, working notes, and license only. It does not contain raw replays, agent logs, API credentials, or analysis data with unclear ownership.
