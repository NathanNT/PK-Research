# PokerStars Protocol Reversal

**Independent reverse-engineering research into the PokerStars desktop client, its communication pipeline, and the reconstruction of structured poker observations.**

The project studies how data travels through a native desktop application: from transport and local inter-process communication to plaintext parsing, message dispatch, application state, and persistent storage.

Its longer-term purpose is experimental: build a reliable foundation for studying player behavior, game theory, predictive models, and poker learning tools. Automated gameplay is not the project's objective.

> This repository is the public overview of an ongoing private research project. It does not distribute the client, instrumentation, raw captures, or a runnable collection system.

## Why this project?

Understanding a protocol is more than finding message identifiers. To turn a stream into useful research data, each observation needs a meaning, a unit, an owner, and a place in the sequence of events.

For example, an amount observed after an action is not necessarily the stack available before that action. A message that names a player does not automatically identify the table or hand. A reconstructed amount to call does not prove that raising is permitted.

The central question is therefore:

**Can a recorded source be transformed into a correctly attributed, reproducible observation of a player's decision, without inventing missing information or using future events?**

## Three connected workstreams

| Workstream | Purpose | Expected output |
|---|---|---|
| **ReverseProtocol** | Identify message families, subscription structures, readers, routing, fields, and their consumers. | Versioned protocol contracts with evidence and explicit uncertainty. |
| **Connecteur DB** | Decode and attribute observations, preserve their provenance, and expose reliable records to downstream consumers. | Replayable events and decision contexts stored in SQLite. |
| **MiniClient** | Study and implement an independent client path, including framing, logical connections, authentication, subscriptions, and state synchronization. | A progressively validated client implementation; each connection stage is qualified separately. |

These workstreams share findings, but their success criteria differ. A documented reader is not a working subscription, and a successful database replay is not proof of continuous operation against a live service.

## Communication and data pipeline

The research follows several layers rather than treating every captured buffer as a complete network message:

```text
Transport and local IPC
          |
Fragment reconstruction and decryption boundary
          |
Plaintext readers and application framing
          |
Message dispatch / subscription routing
          |
Table, hand, player, and tournament state
          |
Attributed events with source references
          |
SQLite -> replay -> statistics and research models
```

Some internal messages are constructed locally. Others carry subscription images or incremental updates. Their identifiers and paths belong to different namespaces and cannot be combined into a single count without qualification.

## Findings and implemented components

The private research has established and documented:

- The active local transport and IPC path for analyzed client builds.
- Reconstruction of fragmented TLS records and identification of an inbound plaintext boundary before application-level parsing.
- Parts of the message-processing pipeline, including native readers, virtual tables, type information, dispatchers, and subscription callbacks.
- Selected table and tournament structures covering identities, seats, hand state, actions, cards, amounts, and configuration.
- The distinction between initial subscription images, later updates, and state invalidation across generations.
- Offline replay and source-to-database checks for selected recorded sequences.
- Decision-context reconstruction and experimental behavioral modeling from observed public actions.

These are bounded findings. A result verified on a named build or recorded sequence is not presented as a universal protocol specification.

## What data matters?

The useful unit of analysis is a **decision with its preceding context**, rather than an isolated action or a large volume of raw traffic.

| Information | Research role |
|---|---|
| Player, table, hand, seat, session generation, and event order | Keep observations attached to the correct situation. |
| Game format, betting rules, blinds, antes, and amount units | Make comparisons between compatible situations. |
| Position, active opponents, stacks, contributions, board, and pots | Describe what was known before the action. |
| Amount to call, offered actions, raise bounds, and decision freshness | Distinguish actual opportunities from unavailable choices. |
| Observed action and amount | Provide behavioral observations and labels. |
| Tournament population, payouts, bounty context, and entry conditions | Support contextual analysis where those fields are qualified. |
| Capture gaps, source references, decoder versions, and corrections | Make results auditable and prevent duplicate or misleading observations. |

Revealed cards and hand outcomes may support later analysis, but must not leak into the input describing an earlier decision. Hidden opponent cards are not a collection target.

## Methodology

### Start with existing evidence

Research begins with recorded findings, parsers, traces, tests, and work from related components. New analysis should resolve a specific missing connection rather than repeat an inventory.

### Follow the complete chain

A protocol claim is traced through the relevant producer, reader, routing condition, state update, and consumer. Type names and RTTI are useful clues; they are not sufficient evidence of field semantics or network reachability.

### Keep claims versioned

Build identity, binary hashes, relevant code locations, recording references, and known limits accompany technical findings. Offsets and layouts are not silently transferred between builds.

### Validate against recordings

Offline replays exercise decoding, attribution, storage, and consumption. Negative cases check missing context, incomplete captures, conflicting identities, stale state, and repeated events.

### Preserve uncertainty

Findings distinguish **confirmed**, **probable**, and **unverified** behavior. Missing values remain unknown. Derived values remain labeled as derived, and partial observations do not become complete hands.

## Tools and engineering

The work combines:

- **Static analysis:** historical IDA work and current REA/Ghidra analysis.
- **Runtime investigation:** Frida instrumentation, Windows API and IPC analysis.
- **Data engineering:** Python decoders, SQLite persistence, deterministic offline replay.
- **Automation and validation:** PowerShell tooling, finite fixtures, source manifests, and regression tests.

## Current limits

The work is ongoing. Message identification and routing coverage do not mean that every field is decoded or every game variant is supported.

In particular, complete field attribution, legal-action delivery, monetary units, build compatibility, and continuous operation require their own evidence. The project does not claim exhaustive protocol coverage, production readiness, profitable play, or a complete model of player behavior.

The existing behavioral models are exploratory. Predicting observed actions is different from proving that an action is optimal.

## Publication scope

This public repository documents the purpose, architecture, methodology, and scope of the research. The implementation and detailed evidence remain private.

Unpublished material includes raw captures, instrumentation, executable offsets and signatures, client-specific protocol contracts, and operational configuration. A public research dataset is a possible future direction; no such dataset is released here.

There is currently no public installation procedure or runnable release.

## Project context

This is an independent personal research effort involving the desktop client distributed to end users. The research does not rely on privileged, administrative, internal, or server-side access.

The project is not affiliated with or endorsed by PokerStars. Product names and trademarks belong to their respective owners.

## Contact

For discussion of the methodology, architecture, or research findings:

**[nathantbl1@gmail.com](mailto:nathantbl1@gmail.com)**
