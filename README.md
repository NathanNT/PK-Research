<p align="center">
  <img src="assets/research-banner.png" width="800" alt="PK Research">
</p>

PK Research began with an investigation of the PokerStars desktop client:
how incoming messages become the cards, actions and amounts shown at a table.
The project now includes tools for observing and replaying hands, a headless
client, and Needle, a model that learns how individual players behave.

The poker research builds on these recorded hands. Needle uses each player's
history to predict their next action, then searches for a response in a
simulated continuation.

## Project ecosystem

| Component | Purpose | Status |
| --- | --- | --- |
| **PK HUD** | Table observation, player statistics, hand history, replay and decision analysis. | Implemented |
| **PK Client** | Protocol connection and table-state reconstruction without the desktop interface. | Complete for the project's needs |
| **Reversal Protocol** | Protocol mapping needed for table observation and data collection. | Complete for the project's needs |
| **DB** | Research database, with the indexer/dispatcher as one of its components. | In development |
| **[Needle 0](docs/models/needle-0/MODEL.en.md)** | First supervised model of player behavior. | Implemented supervised reference, evaluated offline |
| **[Needle 1](docs/models/needle-1/MODEL.en.md)** | Design for player memory, inferred ranges and optional input fields. | Earlier architecture, with components carried into Needle 2 |
| **[Needle 2](docs/models/needle-2/MODEL.en.md)** | Individual behavior prediction and native decision search. | Implemented, strategic evaluation in progress |
| **[Needle 3](docs/models/needle-3/ARCHITECTURE.en.md)** | Needle's opponent model combined with a CFR reference strategy. | Proposed architecture |
| **[Needle 4](docs/models/needle-4/STATUS.en.md)** | Research into strategic capabilities beyond Needle 3. | Research, no selected architecture |

## Needle

Needle predicts how a particular opponent is likely to act, using the current
hand and that player's earlier decisions. A native C++ engine then simulates
the hero's options against those predictions and compares their expected
value.

Needle 2 is the current implementation. It represents all 1,326 private-card
combinations, updates opponent ranges as actions are observed and queries
the model during the search. Training uses recorded decisions, without
reinforcement learning or self-play. Poker Arena is used to test the resulting
player in controlled matches and to look for exploitable weaknesses.

Needle 3 would combine that model with a CFR reference strategy. The idea is
to rely less on individual predictions when there is little evidence to
support them. Its first proposed version is a heads-up, 100 bb, no-rake
demonstrator. Needle 4 explores what could come after it. Both remain research
and design work, with no demonstrated playing strength.

## From messages to hands

Reversal Protocol and PK Client cover the protocol and client behavior needed
for the project's use. The mapping follows data through transport, decoding,
message dispatch and table state. Actions, cards and amounts must be attached to the right
table, hand and player before they can be used by Needle. Missing information
stays unknown, and recordings can be replayed to check the reconstruction.

<p align="left">
  <img src="assets/communication-pipeline.svg" width="720" alt="How observations move from the client protocol into table state and storage">
</p>

The analyzed Windows builds include 83.0.8.4 and 83.3.2.0. Findings belong to
the build and recordings on which they were checked. The network path includes
TLS, CommLib Pike, LZHL, Stream0 and CommMsg decoding.

<p align="left">
  <img src="assets/network-pipeline-stages.svg" width="720" alt="The stages of network decoding">
</p>

## The interface

These screenshots show the application connected to an isolated demonstration
server. Everything displayed, including players, hands, recommendations,
counts and amounts, is synthetic.

<p align="left">
  <img src="assets/table-observation.png" width="640" alt="Table observation and decision review using synthetic hands">
</p>

The table view brings the hand, player history and decision analysis together.

<p align="left">
  <img src="assets/database-provenance.png" width="640" alt="Collection and data-quality view using synthetic records">
</p>

The collection view shows which observations are available and where fields
are missing.

## Working with observed hands

Each training example pairs an action with the history available before it.
Cards revealed later can help train or evaluate the model, but must not appear
in its earlier inputs.

Showdowns create a particular difficulty: the hands we get to see are not a
random sample of all hands played. The Needle documents cover this problem
alongside the model, experiments and results.

## This repository

This is the public documentation of a personal research project. The code,
player records, model weights, raw captures and private configuration remain
in the private workspace. The project supports offline study and simulation
and does not automate betting actions.

The project is not affiliated with PokerStars.

## Contact

**[nathantbl1@gmail.com](mailto:nathantbl1@gmail.com)**
