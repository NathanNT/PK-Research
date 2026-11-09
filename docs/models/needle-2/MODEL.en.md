# Needle 2: learning how opponents play

Needle 2 is a supervised player model coupled to a native decision search.
It learns from observed behavior, estimates what an opponent is likely to do
and compares the hero's legal options against those predictions. It uses
neither reinforcement learning nor self-play.

This account describes the private implementation reviewed on
9 November 2026. **Confirmed** identifies code or measurements within their stated scope.
**Probable** identifies a justified interpretation or estimate.
**Unverified** identifies a capability that has not been demonstrated.
Measurements come from versioned internal reports. This public article is
not an independent reproduction of those experiments.

[Project overview](../../../README.md) ·
[Needle 1](../needle-1/MODEL.en.md) ·
[Needle 3](../needle-3/ARCHITECTURE.en.md) ·
[Needle 4 research](../needle-4/STATUS.en.md)

## From a player's history to an action prediction

**Confirmed.** Needle 2 keeps the history-based approach of Needle 1 while
changing the representation of cards and actions. Typed embeddings describe
the available fields. A current-hand encoder processes the public situation
and earlier actions, while a history encoder summarizes the player's past.
FiLM modulation and population profiles connect those representations.

The memory contains up to 256 previous hands. Slow and fast components
separate longer-term tendencies from recent behavior, with a dedicated
32-hand recent window. Time2Vec and explicit unknown-age handling carry
temporal information without assigning a fabricated date to missing history.

Private hands are represented by all 1,326 unordered combinations of two
cards, composed from 52 card embeddings. The output is one joint distribution
over 15 outcomes: fold, check, call, six bet classes and six raise classes.
The amount classes describe chips added divided by the pot before the action.
That convention matters when connecting the model to a solver that expresses
a raise as a total street commitment.

The default network has width 128, four attention heads, two layers and a
feedforward width of 512, with dropout 0.1. The default training setup uses
AdamW at 3e-4, weight decay 0.01 and three epochs per member. These are
implemented settings, not a measured optimum. Smaller experimental runs
must retain their own configuration.

## What the observations can establish

**Confirmed.** The corpus comes from attributed network observations in the
authorized local project. An observed action is a behavioral target. It is
not evidence that the action was strategically good.

The qualification pipeline grades hands and exports A-grade hands by default.
Ambiguous identities, incomplete histories and inconsistent economic states
remain visible instead of being repaired silently. Inputs must respect the
declared information boundary. Later cards, outcomes and the target action
cannot become features of an earlier prediction.

A historical v3 export contained 58,342 hands and 630,456 decisions. Its
chronological partitions comprised 40,994 training hands, 5,771 validation
hands, 5,773 calibration hands and 5,804 test hands. Those are dated export
counts, not the current collection total. Chronological separation alone
does not establish generalization to previously unseen players.

**Confirmed.** Subsequent v4, v5 and v6 qualification policies changed the
admission of observed cash-game deductions and side pots. The current
reviewed preparation targets a stricter v6 contract and tokenizer v2.
An old export, cache or checkpoint keeps its original version.

**Probable.** A main-pot discrepancy can reflect side-pot accounting rather
than rake. Admission therefore depends on the relevant causal and settlement
checks. Even an admitted deduction does not establish a universal rake rate
or cap. Reception timestamps also have a narrower meaning than human thinking
time. They measure an observation boundary that can include transport delay.

**Confirmed.** Revealed cards are a selected sample. Call frequencies and
showdowns do not automatically identify the cards that were folded.
This limits what supervised range inference can establish, even with a
large history.

## Inferring ranges without looking into the future

**Confirmed.** The predictor estimates a distribution of the form

```text
p_theta(action, sizing_class | public_prefix, player_history, private_combo).
```

A causal filter updates the weight of each possible private combination
using the likelihood of earlier actions. Public-card blockers remove
impossible combinations, and the remaining weights are normalized. The
training path recomputes these beliefs with an EM-like detached stage.
An unknown amount is handled by marginalizing the sizing classes of the
observed action.

The supervised objective and the filter use the same action likelihood.
This keeps their definitions consistent, but does not prove that the hidden
range is correct. The observation-derived legal mask is conservative and
belongs at the output. It must not supply privileged input features or turn
an unknown label into an invented action.

## Training and uncertainty

**Confirmed.** A qualified frozen export is converted into a supervised
cache, then used for training, calibration and bundle creation. Numerical
normalization is fitted on training data only. Past-hand tokens are stored
once and referenced by causal examples.

The training unit is a hand, with decision microbatches. Multiple hands per
optimizer step are supported, although changing that setting changes the
optimization and needs its own comparison. Checkpoints retain optimizer
state, random state, progress, configuration and provenance fingerprints.
Compatibility guards cover tokenizers, vocabularies, ensemble members and
receipts. Exact continuation has been checked on CPU within its declared
scope.

The implemented uncertainty path combines three independent members,
temperature scaling and calibrated dispersion. Calibration uses its
reserved split. A partial run or missing calibration remains explicit.
Ensemble agreement, prediction calibration and Monte Carlo intervals
measure different things. None alone certifies the error of a strategic
value estimate or bounds exploitability.

## How the decision search uses the model

**Confirmed.** The native C++ worker samples plausible opponent cards from
the supplied ranges, advances legal public states and queries predictions
at the nodes actually reached. Each hypothetical actor sees its own sampled
cards and the public history. It never receives another actor's hidden
cards or the unrevealed board.

The search handles betting rules, contributions and side pots. Its basic
objective is expected chips. ICM utility is available when the required
tournament information exists. Missing field information requires an
explicit fallback.

A typical bounded action menu contains pot fractions 0.25, 0.5, 0.85, 1.25
and 2, plus all-in, projected onto legal amounts. Samples, raise caps and
menu restrictions are part of the policy being evaluated. The hero's later
play uses response tables and heuristics. It is not a learned equilibrium
strategy, a CFR solve or an exact best response in unrestricted Hold'em.

Grouped searches can reuse qualified public-node calculations. Incomplete
searches are marked instead of being admitted as complete oracle rows.
Global decision budgets and a smaller continuation ensemble are optional
variants whose effects need separate measurement.

**Confirmed.** One historical 16-sample profile took about 1.7 seconds per
decision, with network calls accounting for 87 to 91 percent of the time.
Other configurations are substantially slower. That profile is not a
throughput promise for a future model or benchmark.

## What the experiments say

**Confirmed.** Technical qualification covers encoding, finite gradients,
causal ranges, checkpoint continuation, memory, native callbacks, rules
and Arena integration. One historical selection passed 131 tests.
These checks establish specific software properties, not playing strength.

The B1 trial is a restricted model: one member, one epoch, the historical
v3 corpus and tokenizer v1. Its final calibration used 57,539 decisions.
It must not stand in for a complete current Needle 2 ensemble.

A paired simulator study found regressions against Needle 0. The following
intervals are individual 95 percent intervals for B1 minus Needle 0.

| Measurement | Difference | 95 percent interval |
| --- | ---: | ---: |
| HU cash, chance-adjusted winnings | -336.45 bb/100 | [-526.88, -164.87] |
| Six-player cash, chance-adjusted winnings | -642.83 bb/100 | [-1088.40, -247.92] |
| Controlled river regret | +5.92 bb/decision | [+3.67, +8.48] |
| Controlled adaptation regret | +6.32 bb/decision | [+3.35, +9.98] |

The regressions remained significant after Holm correction over nine primary
contrasts. Higher regret is worse. The cash panels used programmed opponents,
including weak controls. B1 still had positive absolute winnings there, but
these figures do not estimate strength against humans. The short ABR
contrast was inconclusive.

**Unverified.** The cause of the regression has not been isolated.
Undertraining, representation changes, continuation quality and incomplete
search are hypotheses. The result does not establish the performance of a
newly trained current bundle.

**Probable.** The next useful step is to qualify the corpus, tokenizer and
bundle together, measure complete decisions under a declared budget and
separate training changes from search changes. Consistent strength against
independent strong bots or humans remains **unverified**.

Needle decisions carry `automation: false`. The model and Arena adapter do
not connect to a game client or automate betting. This public account contains
aggregate results and design only. Player records, weights, raw observations
and private configuration remain outside the repository.
