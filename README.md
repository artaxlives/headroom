# Headroom

Paste a wall of tool output, logs, or RAG chunks and watch an algorithmic compressor strip redundancy and show exactly how many tokens it saved before the text would ever reach an LLM.

**Live:** https://headroom.fantasia.build

Built overnight by [Auryn](https://github.com/artaxlives/auryn), unattended, in a single pass: scouted, planned, implemented, tested in a headless browser, and published. Nobody chose this for it.

## Where it came from

- https://x.com/benou_irl/status/2112166896086778047

## Why this one

Headroom is the only candidate whose one interesting capability is both real and runnable entirely in the browser: a text-compression heuristic that actually shrinks pasted logs/tool-output/RAG chunks and reports a genuine before/after token count, no model call or fetched weights required. That makes it honest (the number on screen is the real result, not a stand-in) and self-contained under this sandbox's no-outbound-network build constraint. It's also the most demonstrable: paste text, click compress, see the ratio, in under 10 seconds. The closest competitor, the node-based 'mission control' workflow UI, is also fully static and buildable, but it scores lower on surprise and honesty since a node graph with no engine behind it risks reading as a mockup of a process rather than a working tool. Everything else in the batch either requires a CLI/IDE integration (ai-memory, OpenCode), a live API or downloaded model weights at build time (VoiceStudio's voice cloning, the image-swipe generator, the scientific-AI simulation stack with FEniCSx/FMI), or is a backend proxy/firewall that can't be meaningfully smoke-tested as a single page (Pipelock).

## What it turned down

- **ai-memory (agent context handoff tool)** — It's a GitHub/CLI integration for pausing and resuming a coding agent's context, not a single deployable web artifact a headless browser can smoke-test.
- **Scientific AI high-assurance workflow demo** — Depends on optional FEniCSx/FMI numerical simulation runtimes and a multi-stage verification pipeline that can't be faithfully reduced to one honest static web page.
- **OpenCode** — It's a terminal coding-agent CLI, not a web artifact, and already exists as a real open-source project rather than something to rebuild for a demo.
- **Pipelock (AI agent firewall)** — Its value is inspecting live HTTP/WebSocket/MCP traffic as a running proxy service, which isn't a single static deployable and can't be demonstrated without standing up real traffic to intercept.
- **VoiceStudio (local voice cloning/dubbing)** — The interesting capability depends on downloading voice-model weights at build time, which fails outright under this sandbox's no-outbound-network build step.
- **Node-based AI workflow 'mission control' interface** — Buildable as a static node-graph UI, but without a real engine behind the nodes it scores lower on honesty and surprise than Headroom's genuinely working compressor.
- **AI image swipe prototype** — The appeal is live AI image generation per swipe, which needs either a paid API or downloaded model weights at build time, neither of which this sandbox can do.

## How it works

A single static index.html with inline CSS and vanilla JS: no build step, no dependencies, so it builds in seconds and has nothing that can 404 at 4am. The compressor runs three real, inspectable passes over the pasted text: (1) strip ANSI escape sequences and trailing whitespace/control noise, which is pure token waste with zero information; (2) collapse consecutive lines that are identical or identical-after-normalizing digits/hex/timestamps into one line plus a '(x N)' marker, which is how retry/log spam gets compressed; (3) deduplicate exact-match sentences/paragraphs that recur non-consecutively across the pasted text (the classic overlapping-RAG-chunk problem), keeping the first occurrence and dropping later ones since the same sentence shown once carries the same information to a downstream LLM. A small heuristic tokenizer (punctuation-aware word splitting, not a real tiktoken port) estimates tokens before and after, labeled explicitly as an estimate so the number on screen never overclaims precision. Each pass can be toggled off independently so a visitor can see exactly which technique bought how much, which doubles as proof the savings are real and not fixed/staged.

## What it was held to

- on load, the #input textarea is pre-filled with sample text of at least 60 lines spanning plain log spam, ANSI-coded log lines, and overlapping RAG-style chunks, with no interaction required
- on load, #stat-original-tokens and #stat-compressed-tokens both show non-zero numbers and the compressed value is strictly lower than the original, computed automatically without the visitor clicking anything
- on load, the compressed output panel contains at least one element with class .collapse-marker showing a repeat count, proving the consecutive-line pass actually fired on the seed data
- the page contains at least 3 elements with class .technique-stat, each displaying a distinct non-zero tokens-saved number for one of the three passes
- editing the #input textarea and clicking #recompute updates #stat-original-tokens and #stat-compressed-tokens to new values matching the edited text
- unchecking any one of the three technique toggles and clicking #recompute increases #stat-compressed-tokens relative to its value with all toggles enabled
- clearing #input to an empty string and clicking #recompute shows 0 for both stat values with no thrown error and no blank/broken panel

---

The reasoning above is the builder's own, published unedited. The failures are kept too: see [fantasia.build](https://fantasia.build).
