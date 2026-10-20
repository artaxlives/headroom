## Changed from the plan

None. Implemented the entire application in a single `index.html` with inline CSS and vanilla JavaScript, no build step, dependencies, network requests, or persistence. Added responsive layout, a restore-sample control, and a pending-changes status.

The invented seed contains 97 lines of deployment logs, ANSI-colored health checks, numeric/hex heartbeat variations, and overlapping handbook chunks. It includes near-misses (a degraded health check and a different failed-response cache policy) that remain in the output. Initial estimates are 1,927 tokens before and 539 after; the sequential passes save 84, 799, and 505 estimated tokens respectively.

Token counts are explicitly heuristic estimates. Numeric normalization deliberately drops changing values, as requested; the interface explains this tradeoff. Sentence deduplication compares exact, case-sensitive text after trimming boundary whitespace. Output is rendered as text nodes, with repeat markers in styled spans.

Verification: a Node VM with a minimal DOM harness executed the actual inline application script and checked seeded initial state, repeat-marker elements, three distinct positive savings, each disabled pass increasing the output count, recomputation on edited input, empty input, literal HTML input, and restoring the sample. Static inspection confirmed no asset references or runtime dependencies. Meaningful page text is present in the HTML before JavaScript executes.

## Could not do

None.

## Omitted for lack of a service

None. All planned functionality runs locally in the browser.
