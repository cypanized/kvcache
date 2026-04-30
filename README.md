# LMCache + NIXL — an interactive explainer

An interactive, single-file HTML explainer that walks through how
**LMCache** and **NIXL** address the cost of long-context LLM inference.

It covers:

- **Prefill vs. Decode** — what each phase actually does, with an animated timeline
- **The waste** — KV cache born → used → tossed, with a per-request "what compute is wasted" view
- **Together: who decides, who moves** — architectural stack (Engine → LMCache → NIXL → backends), with VAST Data shown as a concrete storage backend
- **A cache-hit flow** — sequence diagram across Engine / LMCache / NIXL / Storage
- **LMCache in detail** — what it is, what it does, three example use cases (RAG / shared system prompt / multi-turn chat), plus a live prefix-matching demo
- **NIXL in detail** — buffer-list abstraction, scattered-chunks visualization, two design-win diagrams (zero-copy with the RDMA caveat, async/non-blocking)
- **Storage offload benchmark** — recreated chart from the original talk (TTFT vs ISL, KV computed vs retrieved from VAST Storage), with a link to the source video at the 23-minute mark

## How to view

Just open `lmcache-nixl-explainer.html` in any modern browser. The deck has two modes:

- **Scroll mode** (default): all sections flow vertically, each one rendered at its full design canvas
- **Present mode**: slide-by-slide navigation with arrow keys, dot navigator, and fullscreen support

Toggle between them via the top bar, or pass `?mode=present` / `?mode=scroll` in the URL.

## Source

Built from the talk *"Scaling KV Caches for LLMs — How LMCache and NIXL Handle
Network and Storage Heterogeneity"* by J. Jiang and M. Khazraee (NVIDIA),
published on the PyTorch YouTube channel.

Numbers in the illustrative charts (prefill/decode timing, latency curves) are
synthesized to capture the qualitative shape; the storage-offload benchmark
chart reproduces the values shown on screen during the talk.

## Brand

Styling uses the VAST Data brand palette: deep blue background (`#0a1628`),
navy panels (`#162a4a`), cyan accents (`#00d4ff`), white text, silver muted
text, sharp lines, and grid-based layouts.
