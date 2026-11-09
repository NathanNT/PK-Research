# Needle 3: combining behavioral prediction with a CFR reference

Needle 3 is a proposed architecture, not an implemented playing system.
It would combine an equilibrium-oriented reference strategy with Needle 2's
individualized predictions. The reference would provide a starting policy,
while the behavioral model would support measured deviations against
predictable opponents.

This article describes the design reviewed on 9 November 2026.
**Confirmed** identifies existing code or a measurement in its stated scope.
**Probable** identifies a proposed choice or estimate.
**Unverified** identifies a capability that remains to be demonstrated.
The common CFR bridge and small-game experiments are enabling work. They
do not constitute an integrated Needle 3.

[Project overview](../../../README.md) ·
[Needle 2](../needle-2/MODEL.en.md) ·
[Needle 4 research](../needle-4/STATUS.en.md)

## Start with a laboratory demonstrator

**Probable.** The first version should be heads-up no-limit cash at 100 bb,
without rake or ante. Blinds would be 50/100 integer units and stacks 10,000,
reset every hand. One chip would represent 0.01 bb. This is more precise than
saying only “100 bb”, since a 1/2-blind game does not admit the same integer
bet sizes.

That scope covers little of the observed cash 6 to 9-player and tournament
population. Its purpose is to qualify strategy interfaces, computation cost
and resistance to tested attacks before broadening the claim.
**Unverified.** No integrated Needle 3 winnings or exploitability result
has been established.

The design reuses Needle 2's typed observations, memory, full-combo
predictions, calibration and native rules. Arena, replayable evidence,
chance-event variance reduction and HU attack tools already exist.
Their availability does not imply that the combined strategy is strong.

## Qualify the reference before building around it

**Confirmed.** The selected research target is the released HU100 v0.5.3
reference from [CFR-NLHE-6max](https://github.com/dberweger2017/CFR-NLHE-6max),
at source revision `74f1be71c1b0f4851dac5e701862e22a1290da23`.
Its admitted heads-up domain is narrower than the repository's name.

**Probable.** The first implementation should preserve the reference's
169 preflop classes, 50 postflop buckets per street, action menu and bounded
translation. Changing those while integrating it would make the comparison
harder to interpret.

**Confirmed.** An audit of the pinned source showed that hands in the same
current K50 bucket share the final action distribution at the same public
prefix and configuration, including translation and fallback. This proves
a representation property. It does not establish strategic weakness.

**Unverified.** The selected assets, installed tables, natural-state support,
latency and playing strength still require qualification. A policy called
CFR is not automatically close to equilibrium.

**Probable.** The early gate, R0, must check artifact identity and rules,
then measure direct hits, translations, missing or zero-mass fallback,
legality, startup and warm cost. HU LBR, bounded ABR and matches against
admissible Needle references and an independent opponent family would
follow. Acceptance thresholds should be frozen before reserved evaluation.

The gate comes before expensive integration. Failure or an unaffordable
measurement leaves the reference rejected or inconclusive. It does not
justify replacing it silently and declaring Needle 3 complete. An initial
two-CPU-hour tranche is a planning limit, not a measured runtime or standing
permission to compute.

## Make both policies describe the same game

**Confirmed.** Needle's sizing labels describe chips added relative to the
pot before the action. A CFR action may describe the total street commitment.
Equal numerical fractions can therefore mean different bets.

**Probable.** Both menus should be converted into concrete legal integer
amounts. The adapter would preserve probability mass, deduplicate equivalent
all-ins and expose every translation. A clamp, nearest-size replacement or
renormalization must not change the policy silently. Increasing a native
menu limit is an explicit interface change.

Ranges should retain exact card combinations, blockers, provenance and
declared joint dependencies. Multiplying opponent marginals does not
automatically produce the right joint distribution. Hypothetical queries
must respect what each actor could know.

Python would handle observations, memory, neural inference and orchestration.
C++ would retain rules, contribution accounting, terminal values and bounded
search. Batching and cache reuse would apply only where equality has been
qualified.

The original translation is the first reference. Pseudo-harmonic translation
is an alternative for a controlled comparison. Neither makes the abstract
game strategically identical to the real game.

## Use a complete policy, not a fresh coin at every action

**Probable.** Let B be the CFR reference and E the exploiting policy.
E would use Needle's ranges and predictions in native search, with reference
hero continuations. Both components must expose their full conditional
action distributions, not just a sampled action.

```text
Public history and own cards
             |
Domain, identity and artifact checks
             |
     B: CFR reference
     E: Needle model and native search
             |
Private component selected once per hand
             |
Legal action and diagnostics
automation: false
```

The proposed exploitation dose depends on distinct prior hands, ensemble
dispersion, recent stability, identity and format support. One candidate is

```text
n_eff = (sum_h w_h)^2 / sum_h w_h^2

reliability =
  n_eff/(n_eff+n0) * exp(-D/d0) * exp(-C/c0) * support

lambda = clip(lambda_max * reliability, 0, lambda_max).
```

The scale parameters must be positive. Missing signals or no effective
observations imply no exploitation. The dose is frozen before the current
private cards. Multiple actions from one hand do not become independent
extra hands.

A gain score would compare deviations with shared samples and subtract
Monte Carlo error, ensemble disagreement and a distribution-shift margin.
**Unverified.** These quantities do not provide a certified bound on
hidden-range error. Selecting the largest noisy estimate introduces bias,
and local gains do not add up to a guarantee for E.

**Probable.** A private component is drawn once for the whole hand, using E
with probability lambda and otherwise B. Redrawing at each action defines
a different policy. After earlier hero actions, the conditional mixture
must reflect which component was more likely to produce that history:

```text
w_E(I) = lambda*r_E(I) /
         [lambda*r_E(I) + (1-lambda)*r_B(I)]

pi(a | I) = w_E(I)*E(a | I) + (1-w_E(I))*B(a | I).
```

Here r_E and r_B are the hero's own action reaches for the candidate hand
at the same prefix. A zero denominator is explicitly unreachable.
Counterfactual queries cannot mutate the real component, memory or RNG.

**Unverified.** This rule is not a safety proof. Reliability is not
exploitability, and B itself has no established equilibrium certificate.
The architecture does not become Restricted Nash Response or safe subgame
solving merely by drawing inspiration from those methods.

## Extend coverage only through a declared boundary

**Probable.** Hands that began with three or more players, cash with rake
and tournaments would stay on Needle 2 in the first version. That includes
a multiway hand that later becomes heads-up. Independent duels cannot
replace a three-player pot, and a HU exploitability statement cannot
qualify a six-player profile.

The first useful extension would be heads-up after folds in six-player
cash, beginning with river and then turn subgames. It needs qualified
entrance ranges, folded-card effects, contribution accounting, rake
scenarios, boundary values and a policy for reaching the subgame.
The restricted first extension would exclude prior all-ins and side pots.

Tournament push/fold with ICM is a separate problem. Existing ICM utility
does not solve multiplayer equilibrium or supply missing field information.

## Cost, storage and feasibility

**Probable.** Large strategy tables and weights should stay outside the
repository as immutable artifacts, with qualified memory mapping where
appropriate. Public schemas, identities and checksums can describe them.
Corruption or incompatibility should cause rejection, not a uniform-policy
fallback disguised as successful loading.

Qualifying an existing HU reference is preferable to immediately rebuilding
a blueprint. External-sampling MCCFR is a possible CPU-bounded replacement
if that choice becomes justified. CFR+ remains suitable for small validation
games. OpenSpiel and postflop solvers are research references subject to
license and scope checks. No third-party general-purpose solver has been
selected and evaluated as Needle 3.

The restricted version was estimated at 18 to 33 person-days, excluding a
qualified current Needle 2 bundle and full evaluation campaigns.
Heads-up-after-folds would add an estimated 12 to 20 person-days.
These are engineering estimates, not delivery promises. Solver and benchmark
cost may exceed the initial envelopes.

**Confirmed.** Small local CFR+ experiments converged near the expected
values in their declared games.

| Experiment | First-player value | Exact exploitability |
| --- | ---: | ---: |
| Kuhn, 5,000 full traversals | -0.0555555663 | 0.0000276879 chips/hand |
| Declared Leduc rules, 2,000 iterations | -0.0856027576 | 0.0000819251 chips/hand |

Kuhn's value is close to -1/18. The Leduc result belongs to its documented
betting convention. Hand-level mixture checks matched explicit plans in
tested Kuhn cases, and constructed translation and allocation checks passed.
None of these results establishes Hold'em convergence, provider strength
or multiplayer equilibrium.

## What would justify construction

**Probable.** Construction would start with qualified Needle references
and R0, then connect concrete menus, native continuation and component
snapshots. Mixture probabilities and counterfactual queries would be
checked before tuning on development opponents. Reserved sessions and
attacks would decide whether broader scope is justified.

The primary first-version metric is the winnings of qualified LBR and ABR
attackers, with uncertainty. These are attack-dependent lower bounds on
exploitability. An attacker failing to win is not an equilibrium certificate.
Paired match winnings are secondary, using the available chance-event
variance reduction.

For independent session differences D in bb/100, the planning approximation
for proving an improvement above a minimum is

```text
sessions approximately
  [(z_(1-alpha/2) + z_(1-beta)) * sd(D) /
   (planned_gain - minimum_gain)]^2.
```

Variance must come from development sessions with the actual memory,
session length, attack training and cost. Comparisons require a declared
multiplicity correction. Decisions within a session cannot be bootstrapped
as independent observations, and a favorable interim score is not a stopping
rule.

**Unverified.** Until those gates pass, Needle 3 remains a design and
construction plan. The CFR bridge and Bot Battle make qualification possible.
They do not establish that the proposed player exists or wins.
