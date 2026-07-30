# PDE Decomposition Strategies

> Part of the miaco docs: **[README](./README.md)** · **[COMMANDS](./COMMANDS.md)** · **STRATEGIES** *(you are here)*

miaco supports multiple decomposition strategies that control *how* a prompt is broken down into structured PDE artifacts. Choose a strategy based on prompt complexity, desired confidence calibration, and your API budget.

## Quick Start

```bash
# Default: single-pass (same as before)
miaco decompose run -p "Build a REST API for user management"

# Iterative: 4-pass progressive refinement
miaco decompose run -p "Design a distributed event-sourcing platform..." --strategy iterative-refinement

# Adversarial: dual-framing with consensus merge
miaco decompose run -p "Migrate the monolith to microservices..." --strategy adversarial-consensus
```

The `--strategy` flag works on both `decompose run` and `continue`:

```bash
miaco continue --pde <uuid> --strategy iterative-refinement
```

## Available Strategies

### `standard` (default)

Single-pass decomposition — the same behavior miaco has always had, now wrapped in the strategy interface.

```bash
miaco decompose run -p "Add pagination to the /users endpoint" --strategy standard
# or simply:
miaco decompose run -p "Add pagination to the /users endpoint"
```

**When to use:** Focused, well-defined prompts where one pass captures the full intent.

**Characteristics:**
- 1 LLM call
- Full fallback chain active (Copilot → codex → claude on rate-limit)
- Fastest and cheapest option

---

### `iterative-refinement`

Four sequential passes, each building on the previous. Resumable engines chain sessions; stateless engines receive the accumulated prior-pass JSON in each prompt:

| Pass | Label | What it extracts |
|------|-------|-----------------|
| 1 | **coarse** | Primary intent + secondary intents |
| 2 | **directions** | Four Directions mapping, context requirements, expected outputs |
| 3 | **actions** | Ordered action stack with dependency edges |
| 4 | **calibrate** | Ambiguity flags + recalibrated confidence scores |

```bash
miaco decompose run -p "Redesign the authentication system to support \
  OAuth2, SAML, and magic links while maintaining backwards compatibility \
  with the existing session-cookie flow" --strategy iterative-refinement
```

**When to use:** Complex prompts with multiple interleaved concerns where a single pass tends to produce shallow or under-specified action stacks.

**Characteristics:**
- 4 LLM calls (sequential)
- Uses session chaining when supported; also works with stateless engines such as Ollama
- ~4× latency and API cost vs. standard
- Fallback chain disabled
- Graceful degradation: passes 2–4 can fail independently without crashing the decomposition

**Example output differences vs. standard:**
- More secondary intents discovered across passes
- Action stack items have explicit dependency ordering
- Ambiguity flags are added in the calibration pass
- Confidence scores are recalibrated after full analysis

---

### `adversarial-consensus` ⚠️ Experimental

Two LLM branches with opposing framings, then merged. Remote engines run them in parallel; local Ollama-backed engines (`ollama`, `opencode`) run them sequentially to limit memory pressure:

| Branch | Posture | Confidence range | Ambiguity stance |
|--------|---------|-----------------|-----------------|
| **optimistic** | Assumes clarity | 0.6–1.0 | Minimal flagging |
| **critical** | Assumes ambiguity | 0.3–0.7 | Exhaustive flagging |

```bash
miaco decompose run -p "Migrate the monolith to microservices with \
  zero downtime, preserve all existing API contracts, and introduce \
  event-driven communication between services" --strategy adversarial-consensus
```

**When to use:** Ambiguous or high-stakes prompts where you want the decomposition to surface blind spots and challenge its own assumptions.

**Characteristics:**
- 2 LLM calls (parallel on remote engines, sequential on local Ollama-backed engines)
- **2× model calls** — simultaneous remotely, sequential locally
- ~1× remote wall-clock latency; ~2× locally because branches are serialized
- No session resume required (branches are independent)
- Fallback chain disabled
- If one branch fails, the other is used alone with a warning
- If both branches return parseable JSON but omit a valid `primary`, miaco synthesizes a conservative fallback PDE from the original prompt and records `mergeApproach: "synthetic-prompt-fallback"`

**Reconciliation rules:**
- **Primary intent:** Branch with higher confidence wins
- **Secondary intents:** Merged + deduplicated by `action::target`
- **Context / Outputs:** Set union across both branches
- **Directions:** Concatenated per direction + text dedup
- **Action stack:** Concatenated + text dedup
- **Ambiguities:** Critical branch listed first, then optimistic, deduplicated

## Adoption Guidance

### Recommended strategies by use case

| Strategy | Status | Recommendation |
|----------|--------|----------------|
| `standard` | **Stable** | Default choice. Full backward compatibility with pre-strategy miaco. Use for focused, well-defined prompts. |
| `iterative-refinement` | **Recommended** | Best candidate for recursive / lineage-heavy decomposition (intention-analyst, parent-child PDE trees). Produces stronger action sequencing, better ambiguity surfacing, and more useful recursive evaluation structure. |
| `adversarial-consensus` | **Experimental** | Use for exploratory dual-framing. Branch prompts now reuse the full PDE schema and malformed parseable branches degrade to a conservative synthetic fallback, but reconciliation quality still depends on LLM output consistency. |

### When to use `iterative-refinement`

- Parent-child PDE lineage decomposition
- Intention-analyst workflows requiring deep action sequencing
- Complex prompts with multiple interleaved concerns
- Prompts where a single pass produces shallow or under-specified action stacks

### When to avoid `adversarial-consensus`

- Production decomposition where reliability is critical
- Prompts where you need deterministic, predictable results
- When a conservative synthetic fallback would be too low-confidence for the workflow

## Decision Matrix

| Prompt characteristic | Recommended strategy | Why |
|-----------------------|---------------------|-----|
| Focused, single concern | `standard` | One pass captures everything; no extra cost |
| Multi-concern, well-defined | `standard` or `iterative-refinement` | Standard may suffice; iterative adds depth |
| Complex, multi-concern, interleaved | `iterative-refinement` | Progressive passes extract more structure |
| Ambiguous or underspecified | `adversarial-consensus` | Critical branch surfaces hidden assumptions |
| High-stakes / needs review | `adversarial-consensus` | Dual framing catches optimistic blind spots |
| Cost-sensitive | `standard` | 1 API call vs. 2–4 |
| Latency-sensitive | `standard` or `adversarial-consensus` | Standard is fastest; adversarial runs parallel |

## Metadata in `meta.json`

Strategy execution is recorded in `meta.json` (schema v6):

```json
{
  "schema_version": 6,
  "strategy": "iterative-refinement",
  "strategy_passes": [
    { "passIndex": 0, "passLabel": "coarse" },
    { "passIndex": 1, "passLabel": "directions" },
    { "passIndex": 2, "passLabel": "actions" },
    { "passIndex": 3, "passLabel": "calibrate" }
  ],
  "strategy_warnings": []
}
```

For `adversarial-consensus`, passes are labeled `"optimistic"` and `"critical"`:

```json
{
  "strategy": "adversarial-consensus",
  "strategy_merge_approach": "concatenate",
  "strategy_passes": [
    { "passIndex": 0, "passLabel": "optimistic" },
    { "passIndex": 1, "passLabel": "critical" }
  ]
}
```

## Error Handling

Strategies degrade gracefully rather than failing outright:

- **`iterative-refinement`:** Only pass 1 (coarse) is fatal. If passes 2, 3, or 4 fail, the strategy continues with the accumulated state from prior passes and records a warning in `strategy_warnings`.
- **`adversarial-consensus`:** If one branch fails, the surviving branch's result is used directly (logged as `mergeApproach: "single-branch-fallback"`). If both branches return parseable JSON but neither contains a valid `primary`, the strategy emits a conservative synthesized PDE (`mergeApproach: "synthetic-prompt-fallback"`). It still throws when both branches fail before producing parseable JSON.
- **JSON parse failures:** `safeParseLlmJson()` strips markdown fences and recovers from common LLM formatting issues before giving up.

Warnings appear in CLI output and are persisted in `meta.json` for later inspection:

```bash
# Check warnings on an existing PDE
cat .pde/2605011430--<uuid>/meta.json | jq '.strategy_warnings'
```

## Limitations

1. **Engine compatibility:** `iterative-refinement` uses session continuity when the engine supports it. On stateless engines, each pass relies on the accumulated JSON embedded in its prompt.

2. **API cost:** `iterative-refinement` makes 4 sequential calls; `adversarial-consensus` makes 2 calls (parallel remotely, sequential on `ollama` and `opencode`). Plan quota and local runtime accordingly.

3. **Fallback chain:** The Copilot rate-limit fallback chain (`--fallback-provider`) is **only active for `standard`**. Non-standard strategies disable fallback to avoid mixing engines mid-strategy.

4. **Automatic selection is scoped to one command:** for `decompose run` you choose the strategy explicitly via `--strategy` — miaco does not auto-detect prompt complexity. Only `decompose artefact` (formerly `decompose composition`, still accepted) routes a strategy on your behalf, from the form of each transcription, and it prints the reason so you can override it.

5. **Determinism:** Multi-pass and dual-framing strategies amplify LLM non-determinism. Two runs of the same prompt with the same strategy may produce different reconciled results.

## Backwards Compatibility

- Omitting `--strategy` uses `standard`, producing identical behavior to pre-strategy miaco.
- Existing PDE trees (meta.json v2–v4) remain fully readable. Strategy fields are additive.
- The `result` field in `pde-<uuid>.json` contains a standard `DecompositionResult` regardless of strategy. Downstream consumers need no changes.
- `continue`, `steer`, and all PDE tree commands work on trees created with any strategy.

---

## 🧭 Navigate

- **[README.md](./README.md)** — the philosophy and the decomposition loop
- **[COMMANDS.md](./COMMANDS.md)** — what you ask each command, and what it gives back
