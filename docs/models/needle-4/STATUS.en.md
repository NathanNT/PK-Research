# Needle 4: research status and the next empirical step

This report records the state of work on 9 November 2026 at 18:57
Europe/Paris. Needle 4 has no selected, validated architecture and no
measured playing strength. The work has produced useful constraints,
counterexamples and evaluation components, rather than a demonstrated
successor to Needle 3.

**Confirmed** refers to code or results in their declared scope.
**Probable** refers to a justified proposal or interpretation.
**Unverified** refers to a capability that has not been demonstrated.
Measurements below come from versioned internal reports unless a separate
recheck is stated.

[Project overview](../../../README.md) ·
[Needle 2](../needle-2/MODEL.en.md) ·
[Needle 3 proposal](../needle-3/ARCHITECTURE.en.md)

## What survived the search for a breakthrough

**Confirmed.** The consolidated research catalogue contains 76 formal
proposals and nine secondary mentions. A later study examined 11
constructions across revealed preferences, hybrid control, market design,
evolution and ecology, information and coding, and system reliability.
Those counts describe the search, not a collection of proven innovations.

**Probable.** No new architecture from that later study had sufficient
support to justify a breakthrough claim. Several constructions required
commitments or payments unavailable at a poker table. Others depended on
hidden information, an unverified limitation of human adaptation, or an
advantage that disappeared when a classical control received the same
information.

One reserve idea generates bet sizes around future changes in legal
reopening rights. Constructed cases show that one chip can alter a later
option. Root-level gain, real coverage and a computational advantage remain
**unverified**. For now, it belongs to action-abstraction refinement.

The planned cross-agent review of that final exploration was not completed
before closure. Authors supplied counterexamples, which should not be
presented as an independent external review.

## C2 found a structural opportunity and an identification limit

**Confirmed.** C2 asks whether a player forced to respond identically with
different private hands leaves an exploitable surplus. The response
constraint must exist. Similar observed frequencies are insufficient.

An audit of the pinned public HU100 v0.5.3 source confirmed K50 fusion
through the final action distribution at a fixed public prefix and
configuration. It did not qualify installed tables, natural-state reach,
strength or profit.

A synthetic river game showed why the same inference cannot be generalized
to humans from censored histories alone. Two worlds have exactly the same
observation law, even when every call reveals cards, yet the same candidate
plan wins in one and loses in the other.

| World | Strong-card mass | Call with weaker type | Call with stronger type | Candidate relative to B |
| --- | ---: | ---: | ---: | ---: |
| F, fused response | 2/3 | 1/2 | 1/2 | +1/24 pot |
| D, distinct responses | 1/3 | 1/4 | 1 | -1/24 pot |

Both worlds produce call-weaker mass 1/6, call-stronger mass 1/3 and fold
mass 1/2. Repeating that observation protocol indefinitely cannot distinguish
them. The witness concerns a declared local collection protocol and class
of conditional ranges. It is not a pair of fully indistinguishable Hold'em
games dealt from a uniform deck.

**Confirmed.** Future adaptation creates a related limit. An opponent
that changes its response tomorrow can share the whole observed past of
a stable opponent. More history alone does not certify a permanent human
information constraint.

A follow-up synthetic study generated 5,760 histories and passed 1,130
checks, including negative cases. Passing checks did not mean winning.
In a future-adaptation family at 20,000 simulated hands, C2 lost in 21 of
128 reserved histories. A maximin implementation given the same C2 class
matched its objective exactly. No unique algorithmic superiority was shown.

**Probable.** The inspectable-provider experiment remains worth pursuing.
The universal human-detection and safe-exploitation claim does not survive.
A narrower behavioral claim might become testable with qualified ranges,
revelation, support, temporal limits and abstention. That human observation
contract remains **unverified**.

## A provider experiment is prepared, not measured

**Confirmed.** The prepared panel contains 12 development and 24 reserved
river roots, legal histories, menus and an exact exogenous joint
distribution. Concrete buckets, final probabilities and certificates still
need to be extracted from selected provider assets. The panel does not
estimate the provider's natural reach ranges.

**Probable.** Its proposed gate requires no illegality or information
leakage, valid numerical controls, and at least 6 of 24 reserved roots with
a corrected lower bound of 0.01 pot or more. A fine opponent without fusion
and a classical maximin with the same constraints provide controls.
Success would establish the declared structural opportunity, not human
profit or winnings per complete hand.

## The common CFR bridge now exists

**Confirmed.** Internal commit `3a0246c59` adds versioned single and batch
distribution queries before the action RNG. It exposes final menus,
concrete amounts, raw float64 weights and normalization, together with
identity, translation and fallback diagnostics. A range-policy adapter
connects it to strength tools, while immutable receipts support C2.

The bridge reuses the original policy reader and converter. It neither
trains a strategy nor implements the C2 LP solver. Exact extraction of
final weights also does not mean bitwise equality to the finite RNG's
discrete sampling law.

The author reported 29 passing house-fixture tests. A separate local recheck
repeated those tests and a single/batch fixture check successfully.
No real provider, image or private table was executed.
Actual artifact identity, provider parity, cold and warm cost, and strategic
strength remain **unverified**.

## Bot Battle is ready for qualification work

**Confirmed.** Internal commit `2261c8dde` delivers the BB0/BB1 foundation.
Metadata admission leads to a frozen paired plan, small house-only sessions,
Evidence and replay, chance-adjusted scoring and grouped analysis.
An owned-process supervisor enforces the supported local execution boundary.

The proposed P0 contains six sessions and 24 table hands for diagnostics.
Conditional P1 adds 20 sessions and 240 hands across rotations.
The combined 264 hands are development work, not a strength ranking.
The cumulative eight-CPU-hour limit is an abort ceiling rather than a
measured runtime.

The author reported 68 distinct passing tests, comprising 32 BB0/BB1,
29 CFR and seven transport tests. These included house-only mini-sessions,
not matches against released opponents.

The admitted reference setup uses blinds 50/100, stacks reset to 10,000,
one-chip units and zero ante or rake. HU and six-player results remain
separate. Conditional menus and adapters outside training scope stay
visible. The available variance reduction corrects chance events, not all
action terms of full AIVAT.

**Confirmed.** The delivered version refuses real WSL, Docker and cgroup
execution because a Windows client's Job Object does not account for the
container's full workload. Real manifests and a qualified accounting and
shutdown path are still needed. Neither the real P0 nor P1 has run.

## Where the work goes next

**Confirmed as work in progress.** At this snapshot, a follow-up task is
building actual provider resolution, owned-container accounting, timeouts,
cleanup and cumulative budget enforcement. Its tests use mocks and house
processes. It is not permission to run a provider or campaign.

**Probable.** That code should be reviewed before selecting real manifests
and checking identity and domain receipts. A separately qualified, bounded
provider smoke would then determine whether P0 can be launched.
Measured runtime and variance would size a later independent strength study.

A VPS/API option is designed but not deployed. The referee and Needle would
remain on a trusted machine, sending only legal synthetic observations to
isolated public bots. An API alone supplies no sandbox. Isolation, latency
and resource accounting still require validation.

The useful milestone is a reproducible improvement against several
independent open reference policies at comparable budgets, with an ablation
showing the contribution of player history. Beating one exploitable
blueprint would support a narrower claim.

**Unverified.** Nothing reported here establishes superiority over Pluribus,
all open bots, a two-to-nine-player field or strong humans. HU LBR and ABR
remain attack-dependent lower bounds, not equilibrium certificates or
six-player guarantees.

This public report contains aggregate results and work status. Player
identities, game records, weights, raw logs, local paths and credentials
remain private. Preparing this publication involved no further training,
bot execution, game-service connection or deployment.
