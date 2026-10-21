<p align="center">
  <img src="assets/research-banner.png" width="800" alt="PK Research: protocol research">
</p>


**Independent reverse-engineering research into the PokerStars desktop client, its communication pipeline, and the reconstruction of structured poker observations.**

Analyzed Windows builds: **83.0.8.4** and **83.3.2.0**. Protocol findings are tied to the build on which they were established.

The project studies how data travels through a native desktop application: from transport and local inter-process communication to plaintext parsing, message dispatch, application state, and persistent storage.

Its purpose is experimental: connect reverse engineering, artificial intelligence, and game theory through player-behavior analysis, predictive models, decision research, and poker learning tools.

> This repository presents PK Research, its applications, technical architecture, and findings from ongoing work on the PokerStars desktop client.

## Interface preview

The images below show the existing application interface running against an isolated demonstration server. **All displayed players, hands, recommendations, counts, and amounts are synthetic.** They illustrate table observation, decision analysis, collection monitoring, and data-quality inspection.

### Attributed table observations

<p align="left">
  <img src="assets/table-observation.png" width="640" alt="Table observation interface with fictitious players, a sample board, and synthetic decision context">
</p>

*Demonstration of table state, player context, and the decision-analysis interface. All figures and outputs are fabricated fixtures.*

### Collection and data quality

<p align="left">
  <img src="assets/database-provenance.png" width="640" alt="Database interface with synthetic row counts, storage figures, and field completeness indicators">
</p>

*Demonstration of collection monitoring and explicit missing-field visibility. The displayed percentages illustrate field completeness using synthetic records.*

## How far can we go?

This project starts with curiosity: how far can we go by combining reverse engineering, artificial intelligence, and game theory?

Reverse engineering lets us understand how a complex client represents and processes a game. AI gives us a way to explore patterns in player behavior and build predictive models. Game theory provides a framework for studying decisions, uncertainty, and strategic interaction.

The ambition is to connect these disciplines and see what becomes possible, from reconstructing game states and learning from observed decisions to experimenting with new models and poker learning tools. Each discovery opens another question to investigate.

It is an independent experimental project, driven by learning, building, and testing ideas. Its direction evolves through the discoveries and experiments made along the way.

## Project ecosystem

### PK HUD

**Done**

The application and HUD, including the observation probe, protocol parser, player metrics, database, logistic-regression behavioral prediction, decision analysis, and LLM advice and explanations.

### PK Client

**Work in progress, demonstrated functionality**

An independent client reimplementation in C++, covering communication, logical connections, authentication, subscriptions, and state synchronization. Integration continues.

### Reversel Protocol

**Partial, sufficient for current needs**

Protocol research and documentation, with **150+ protocol-related functions identified**. The project uses the subset relevant to game-state reconstruction and behavioral analysis.

### Needle-0

**Coming soon**

The research dataset and its associated decision model, built from structured game observations and decision contexts. The aim is to develop and evaluate a high-performing decision model.

PK HUD is the implemented application. PK Client provides the independent C++ client path. Reversel Protocol supplies the technical understanding used by both, while Needle-0 names both the upcoming dataset and its associated decision model. The function count refers to identified code functions.

## Communication and data pipeline

The research follows data through transport, parsing, attribution, storage, and analysis:

<p align="left">
  <img src="assets/communication-pipeline.svg" width="720" alt="Communication and data pipeline">
</p>

Some internal messages are constructed locally. Others carry subscription images or incremental updates. Their identifiers and paths are documented within their respective namespaces.

## Findings and implemented components

The private research has established and documented:

- The active local transport and IPC path for analyzed client builds.
- Reconstruction of fragmented TLS records and identification of an inbound plaintext boundary before application-level parsing.
- Parts of the message-processing pipeline, including native readers, virtual tables, type information, dispatchers, and subscription callbacks.
- Selected table and tournament structures covering identities, seats, hand state, actions, cards, amounts, and configuration.
- The distinction between initial subscription images, later updates, and state invalidation across generations.
- Offline replay and source-to-database checks for selected recorded sequences.
- Decision-context reconstruction and experimental behavioral modeling from observed public actions.

Each finding is documented with its analyzed build, supporting evidence, and validation scope.

## What data matters?

The useful unit of analysis is a **decision with its preceding context**.

| Information | Research role |
|---|---|
| Player, table, hand, seat, session generation, and event order | Keep observations attached to the correct situation. |
| Game format, betting rules, blinds, antes, and amount units | Make comparisons between compatible situations. |
| Position, active opponents, stacks, contributions, board, and pots | Describe what was known before the action. |
| Amount to call, offered actions, raise bounds, and decision freshness | Distinguish actual opportunities from unavailable choices. |
| Observed action and amount | Provide behavioral observations and labels. |
| Tournament population, payouts, bounty context, and entry conditions | Support contextual analysis where those fields are qualified. |
| Capture gaps, source references, decoder versions, and corrections | Make results auditable and prevent duplicate or misleading observations. |

The analysis uses public game observations and the local player's own context. Revealed cards and hand outcomes support retrospective analysis, with decision inputs restricted to information available before the action.

## Tools and engineering

The work combines:

- **Static analysis:** historical IDA work and current REA/Ghidra analysis.
- **Runtime investigation:** Frida instrumentation, Windows API and IPC analysis.
- **Data engineering:** Python decoders, SQLite persistence, deterministic offline replay.
- **Automation and validation:** PowerShell tooling, finite fixtures, source manifests, and regression tests.

## Publication scope

This public repository presents the project's goals, applications, architecture, and research findings. The underlying implementation and detailed evidence are maintained in the private research workspace.

Needle-0 brings together the planned research dataset and its associated decision model, extending the work from protocol observation to data-driven experimentation.

## Project context

This is an independent personal research effort focused on the desktop client distributed to end users, combining client-side static analysis, runtime observation, and offline experimentation.

PokerStars is a trademark of its respective owner. PK Research is an independently developed project.

## Contact

For discussion of the methodology, architecture, or research findings:

**[nathantbl1@gmail.com](mailto:nathantbl1@gmail.com)**
