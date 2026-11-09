<p align="center">
  <img src="assets/research-banner.png" width="800" alt="PK Research: protocol research">
</p>

**Independent research into the PokerStars desktop client, its communication pipeline and the reconstruction of structured poker observations.**

PK Research connects protocol analysis, player modeling and decision research.
The project follows data from transport and local communication through
plaintext parsing, message dispatch, application state and persistent storage.
The private workspace includes a table observation interface, a headless
mini-client, recorded history, replay tools and experimental decision analysis.

The analyzed Windows builds include **83.0.8.4** and **83.3.2.0**. Protocol
findings retain the build and evidence on which they were established.

This public repository presents the project, selected illustrations and its
research design. Implementation sources and detailed evidence remain in the
private research workspace.

## Needle research status

As of 9 November 2026, Needle 2 is an implemented supervised player model
with native decision search. Its [technical account](docs/models/needle-2/MODEL.en.md)
describes exact-card prediction, individual memory, causal ranges and the
training pipeline. It also reports the limitations of B1, a one-member,
one-epoch trial on an older data contract. That trial's measured regressions
do not qualify the strength of a complete current ensemble.

[Needle 3](docs/models/needle-3/ARCHITECTURE.en.md) is a proposed combination
of a CFR reference and individualized exploitation. Its first scope is a
HU100 laboratory demonstrator. Small feasibility experiments and reusable
interfaces exist, but the integrated playing system has not been built.

[The Needle 4 work report](docs/models/needle-4/STATUS.en.md) describes the
research completed, C2's results and counterexamples, the common CFR
distribution bridge and Bot Battle. No Needle 4 architecture has been
selected and validated.

The documents use **confirmed** for code or measurements within their
stated scope, **probable** for justified proposals and estimates, and
**unverified** for outstanding claims. The older Needle 0 and Needle 1
sections below preserve the history of the designs.

## Interface preview

These images show the existing interface running against an isolated
demonstration server. **All displayed players, hands, recommendations, counts
and amounts are synthetic.** They illustrate table observation, decision
analysis, collection monitoring and data-quality inspection.

### Attributed table observations

<p align="left">
  <img src="assets/table-observation.png" width="640" alt="Table observation interface with fictitious players, a sample board, and synthetic decision context">
</p>

*Table state, player context and the decision-analysis interface shown with fabricated fixtures.*

### Collection and data quality

<p align="left">
  <img src="assets/database-provenance.png" width="640" alt="Database interface with synthetic row counts, storage figures, and field completeness indicators">
</p>

*Collection monitoring and missing-field visibility. The displayed percentages come from synthetic records.*

## Research objectives

Poker provides a setting for studying sequential decisions under imperfect
information. The first task is to reconstruct an observation with a known
source, table, hand, player and position in the event sequence. That makes it
possible to study behavior without silently filling gaps or using later
information.

Protocol reconstruction provides the foundation. It decodes messages, tracks
logical connections and subscription updates, and preserves identity across
table and session generations. Unknown values and incomplete hands remain
explicit.

Behavioral modeling uses those observations to predict action probabilities
from the current context and the player's earlier decisions. An observed
action provides a behavioral label, not evidence that the action was optimal.
Decision research builds on that distinction by comparing candidate actions
through card equity, declared opponent policies and expected chip value.
Prediction quality, simulated decision quality and performance in a real
game are evaluated separately.

The existing modeling work includes population and player statistics and
**P6**, an experimental hierarchical logistic-regression model. Native equity
and EV calculations form a separate numerical path. The LLM adviser can
select from evaluated candidate actions and explain its choice. Recorded
replay and rendering checks establish parts of that workflow, not a general
strategic advantage.

Experiments use versioned decoders, source references and deterministic
replay. Inputs follow an explicit information boundary. Current exported
observations precede the received action notification, which does not by
itself prove that every feature preceded the human's choice. Later cards,
outcomes and target amounts stay outside that target's inputs.

The project supports research, offline analysis and learning tools. It does
not automate betting actions.

## Project ecosystem

PK HUD is the observation application. It provides player metrics, durable
history, replay and decision analysis. PK Client supplies the independent
headless protocol path, including qualified transport, subscriptions and
state synchronization. Reversel Protocol records the underlying research.
Coverage and validation remain specific to a build, source and task.

Needle uses the resulting observations to study behavior and adaptation.
Needle 0 became a supervised reference evaluated offline. Its proposed
PPE-Policy extension remains separate from that implementation. Needle 1
specified a more modular architecture, with components subsequently reused
and corrected in Needle 2.

Needle 2 implements individualized prediction and native search.
Needle 3 proposes adding an equilibrium-oriented reference, while Needle 4
remains an exploratory research stage. Implementation, design and strategic
qualification are reported separately.

## Needle-0: Poker Player Encoder

*Historical design overview. See the current status above for later implementation and measurements.*

Needle-0 learns from structured histories rather than assigning a fixed label
to a player. A hand encoder summarizes events within each past hand. A history
encoder combines those summaries in time order. A separate branch processes
the current situation. Together, they can represent how the same player
behaves differently across stack depths, positions and betting contexts.

**PPE-Predict** uses the current observation and player representation to
estimate action probabilities and conditional bet-size distributions.
**PPE-Policy** proposed building on that representation to train a policy and
value function in simulation, starting with small games before restricted
NLHE subgames. This remains distinct from Needle 2's supervised predictor
and heuristic search continuation.

Observed amounts are model inputs. The next action and its amount are targets.
Reliability is measured outside the neural network using held-out performance
on comparable cases. This keeps the first version free of a mandatory neural
confidence head.

An earlier private review verified an export of **9,144 reduced-action examples
from 10 sources** and artifacts from **nine lightweight baseline models**.
These historical figures describe that early baseline review, not the current
corpus or subsequent neural model measurements.
Legal-action masks, sizing labels and independent evaluation groups still have
limited coverage. Two additional cohorts can support temporal pilot studies
of already observed players. They do not establish generalization to unseen
players or strategic profitability.

The earlier inventory of 36 information requirements describes source and
context needs. It is not a vector of 36 scalar model inputs.

The [Needle-0 technical document](docs/models/needle-0/MODEL.en.md) explains
how the architecture works, develops its equations and connects the training
procedure, external reliability layer and implementation in a single account.

## Needle-1: modular player modeling and decisions

*Historical architecture specification. Implemented components and corrections carried forward into Needle 2 are documented separately.*

Needle-1 keeps the causal, history-based approach of Needle-0 and addresses
what limited its effect on decisions. Opponent ranges are inferred from
observed actions instead of being assumed uniform. Predictions reach every
node of the native search, and exploitation is weighted by how reliable each
player profile is.

A versioned field registry separates fundamental, economic and
complementary information. The model therefore runs on services that expose
different fields, such as decision timers, without treating a missing value
as zero. A persistent long-term profile uses the complete history, a
short-term branch follows recent behavior, and both are shrunk toward a
population profile when evidence is limited. The architecture covers cash
games, multi-table tournaments, Sit & Go and heads-up play, from two to nine
players, with prize-equity utility for tournaments.

The [Needle-1 architecture specification](docs/models/needle-1/MODEL.en.md)
describes the data contracts, model, player memory, decision engine,
training pipeline, implementation plan and Poker Arena validation criteria.

## Communication and data pipeline

The original investigation follows the native client from transport and local
communication through in-process readers, dispatch and attributed application
state. The network and connected mini-client paths reuse common state
reconstruction, presentation and SQL persistence components.

<p align="left">
  <img src="assets/communication-pipeline.svg" width="720" alt="Communication and data pipeline">
</p>

Some internal messages are constructed locally. Others carry subscription
images or incremental updates. Identifiers and paths retain their own
namespaces.

### Network decoding

Captured traffic is decoded through the TLS, CommLib Pike, LZHL, Stream0
and CommMsg stages shown below.

<p align="left">
  <img src="assets/network-pipeline-stages.svg" width="720" alt="TLS, CommLib Pike, LZHL, Stream0 and CommMsg">
</p>

## Findings and implemented components

The private work has documented the local transport and IPC paths of analyzed
builds, fragmented TLS reconstruction and reproducible Pike, LZHL and Stream0
decoding on qualified captures. It also covers native readers, dispatchers,
subscription callbacks and selected table and tournament structures.

State reconstruction distinguishes initial images, later updates and
invalidation across generations. Selected recordings have passed replay and
source-to-database checks. These findings support attributed actions, cards,
amounts, seats and decision context within their documented scope.

The observation stack, headless client and numerical decision engine are
implemented components. Complete economic state, hidden-card beliefs, rake,
tournament utility and strategic performance require their own validation.

## What data matters?

The useful unit is a decision paired with the information available before
its declared boundary.

| Information | Research role |
| --- | --- |
| Player, table, hand, seat, session generation and event order | Keep each observation attached to its situation |
| Game format, betting rules, blinds, antes and units | Compare compatible situations |
| Position, opponents, stacks, contributions, board and pots | Describe the available context |
| Amount to call, legal actions, raise bounds and freshness | Distinguish real opportunities from unknown choices |
| Observed action and amount | Supply behavioral labels |
| Tournament population, payouts, bounties and entry conditions | Add context where those fields are verified |
| Capture gaps, source references, reader versions and corrections | Make results reproducible and prevent duplicate or misleading records |

Public game observations and the local player's permitted context are kept
separate from hidden opponent information. Revealed cards and outcomes can
support later analysis without leaking into earlier decision inputs.

## Tools and engineering

The workspace combines historical IDA work with current REA analysis,
targeted Frida instrumentation, Windows API and IPC investigation, network
capture and replay, Python decoders and SQLite persistence. PowerShell tools,
source manifests, finite fixtures and regression tests support reproducibility.

## Publication scope

This repository contains the public overview, existing illustrations, the
historical Needle-0/1 design documents, Needle 2 documentation, the proposed
Needle 3 architecture and a dated Needle 4 work report. Aggregate measurements
retain their scope and version. detailed evidence remains private.
Implementation code, individual game records,
private configuration, credentials and detailed experimental evidence remain
in the private workspace.

This is an independent personal research project. PokerStars is a trademark
of its respective owner.

## Contact

For discussion of the methodology, architecture or research findings:

**[nathantbl1@gmail.com](mailto:nathantbl1@gmail.com)**
