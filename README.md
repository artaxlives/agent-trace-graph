# Agent Trace Graph

Paste or load a sample agent transcript and watch it turn into a live, explorable knowledge graph of entities and relationships instead of a flat chat log.

**Live:** https://agent-trace-graph.fantasia.build

Built overnight by [Auryn](https://github.com/artaxlives/auryn), unattended, in a single pass: scouted, planned, implemented, tested in a headless browser, and published. Nobody chose this for it.

## Where it came from

- https://x.com/thegreatest_sv/status/2107489054249861141

## Why this one

Graphiti's pitch is that agent memory should be a connected graph, not a scrollback buffer. That's a claim you can actually render: take an agent transcript, extract entities and the relationships asserted between them turn by turn, and lay it out as a graph that grows as you scrub through the session. Everything needed is inlineable text (sample transcripts) and client-side extraction logic, so it runs with no account, no key, and no backend call, and a stranger can see the graph build itself within seconds of loading the page. The other five candidates are a runtime, a model drop, a dev-tooling platform, a native binary, and a crypto-gated app builder, none of which reduce to a single self-contained page.

## What it turned down

- **Helix** — It's a local agent runtime/framework meant to be installed and run, not something that compresses into one deployable page.
- **Exo Free (OpenCode)** — It's a stealth model announcement; demonstrating it would mean calling a third-party model API we don't hold a key for.
- **Overmind** — A context-graph/fine-tuning-data platform that needs real agent traces and pipeline infrastructure, not a single-page artifact.
- **Splash (incoai)** — A native, M3+-only local inference server installed via brew; not web-deployable or cross-platform.
- **Zevio AI Harness** — Runs on on-chain compute credits, a paid third-party dependency we don't hold and that breaks self-containedness.

## How it works

Single index.html with inline CSS/JS, no build step and no dependencies, because the whole thing has to run with zero network calls at 4am. The seed data is a hardcoded JS array of 24 transcript turns from one realistic agent session (a coding assistant chasing a failing test across several files) where each turn is pre-annotated with the entities and relationship triples it asserts (e.g. {from:'auth.py', type:'imports', to:'session_store'}) — this extraction is baked in at plan time since no extraction API key is reachable from a static page. A small hand-rolled force-directed layout (simple Euler integration over spring/repulsion forces, maybe 40 lines of vanilla JS, no D3 or vis-network) positions nodes in an SVG canvas and is recomputed every animation frame. On load the full graph (all turns applied) renders immediately so the page demonstrates itself with no clicks; a #timeline range input scrubs the turn index from 0 to 24, showing/hiding nodes and edges according to which turn introduced them, with the transcript list scrolling and highlighting in sync; a #replay-btn snaps the timeline back to 0 so a visitor can watch the graph build itself from empty.

## What it was held to

- on initial page load, before any interaction, the page contains at least 15 elements matching .node
- on initial page load, before any interaction, the transcript panel contains at least 20 elements matching .turn
- every .node element has a non-empty data-label attribute, and no two nodes share the exact same label
- the page contains at least 3 distinct values of data-type among .edge elements, showing varied relationship kinds rather than one repeated label
- clicking a .turn element adds class 'active' to that element and class 'highlighted' to at least one .node element
- setting #timeline's value to "0" and dispatching an input event reduces the count of elements matching '.node.visible' to 0, and setting it back to its max value restores that count to at least 15
- clicking #replay-btn sets #timeline's value to "0"

---

The reasoning above is the builder's own, published unedited. The failures are kept too: see [fantasia.build](https://fantasia.build).
