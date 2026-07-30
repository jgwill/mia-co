# 🧭 miaco Commands — what you ask, what you get

A field guide to the miaco surface, written from the user's chair: *what you want → what you type → what comes back*. For the philosophy and the decomposition loop, start at the **[README](./README.md)**. For the decomposition strategies in depth, see **[STRATEGIES.md](./STRATEGIES.md)**.

Every command prints its own options with `miaco <command> --help`. This page is about the *experience*, not the flag list.

Anywhere a command takes `--pde` or `--parent`, hand it either the bare UUID or the PDE folder name exactly as it sits on disk (`2607271430--<uuid>`). Tab-complete the folder, paste it, move on.

---

## The decomposition loop

These are the commands you move through, in order, to take a raw prompt all the way to actionable structure.

### `decompose` — turn a prompt into structure

> *"I have a tangled prompt. Lay it out for me."*

```bash
miaco decompose run -p "<your prompt>"
miaco decompose run -p @./prompt.md           # read the prompt from a file
miaco decompose run -p "<prompt>" --strategy iterative-refinement
miaco decompose list                          # see past decompositions
miaco decompose get <id>                      # reopen one by id
```

**You get back** a PDE folder holding your prompt split into primary intent, secondary intents, the Four Directions, an ordered action stack, and — crucially — **ambiguity flags** naming what was vague. Choose *how* it decomposes with `--strategy` (see [STRATEGIES.md](./STRATEGIES.md)) and *which model* with `--engine`.

> 🌸 This is where the wish becomes a map. The flags it raises aren't complaints — they're the next questions worth asking.

---

### `decompose artefact` — read what you recorded

> *"I recorded hours of thinking out loud. Show me what's in there."*

```bash
miaco decompose artefact ./my-recording             # read it, offline
miaco decompose artefact ./my-recording --segments  # show the moves inside each one
miaco decompose artefact ./my-recording --index 3 --full
miaco decompose artefact ./my-recording --run       # decompose each one for real
```

**`composition` still works, and always will.** It was this command's name until the
name was corrected, so `miaco decompose composition ./my-recording` does exactly
what the line above it does — same command, two names. What you point it at is a
folder of recordings and their transcriptions: an **artefact**. An artefact may
*become* a composition; it does not start as one. The manifest inside it is still
called `composition.json`, and nothing about the file changed.

**You get back** every transcription in the artefact read as its own unit: what form it takes, what its label declared, how many segments it divides into and what move the speaker makes in each, likely mishearings flagged as advisory, and the strategy that form calls for — a request routes to `standard`, a long monologue to `iterative-refinement`, more than one voice to `adversarial-consensus`. Override the routing with `--strategy`.

That read costs nothing: no engine, no network, no API key. It runs on a phone in a field. Add `--run` to decompose each transcription through an engine — one PDE per entry, `--dry-run` first to see what would be sent, `--parent` to nest them all under a tree you already have.

**It reads your artefact; it never writes to it.** Everything produced lands in `.pde/`, carrying provenance back to the exact transcription it came from — including which folder it was read from. The artefact folder belongs to whoever recorded it and comes back untouched.

> 🌸 The recording already knew what it wanted — it just said it in one long breath, walking. This is the command that listens all the way to the end before it answers.

---

### `qmd` — search your shared memory

> *"What do we already know about this?"*

```bash
miaco qmd search "structural tension"     # fast keyword (BM25) lookup
miaco qmd query  "how did we resolve auth migration"   # deeper semantic search
```

**You get back** hits from your shared markdown knowledge base. `search` is fast and literal; `query` expands and reranks for meaning. This is the memory miaco draws on to resolve what a decomposition couldn't settle on its own.

---

### `qmd-inquiry-decompose` — resolve a PDE's ambiguities from memory

> *"Take the blind spots you flagged and go find answers."*

```bash
miaco qmd-inquiry-decompose run --pde <uuid>          # search memory for each facet
miaco qmd-inquiry-decompose formulate --pde <uuid>    # let the session write the queries first
```

**You get back** an enriched artifact beside your PDE: the decomposition's intents and **flagged ambiguities** turned into searches against your memory — keyword search for the intents, semantic search for the ambiguities where meaning matters most. This is the step that closes the loop between *"this is unclear"* and *"here's what we know about it."*

> 🌸 The decomposition asks; the memory answers. The ambiguity was never a dead end — it was the doorway.

---

### `clarify` — settle the obvious placeholders

> *"Resolve the parts that don't even need research."*

```bash
miaco clarify run --pde <uuid>
```

**You get back** a `pde-clarifications.md` beside the PDE, resolving the plainly-fillable gaps and placeholders so the remaining tension is real, not just unstated.

---

### `continue` — resume the work and let the engine build

> *"Pick up where we left off and actually do it."*

```bash
miaco continue --pde <uuid>                       # reopen the interactive session
miaco continue --pde <uuid> --steps clarify,qmd-inquiry-decompose,stc
miaco continue -p "<new prompt>" --parent <uuid>  # start a fresh child, then continue
```

**You get back** the same engine session reopened — optionally after auto-running a chain of follow-up steps — so you continue a train of thought instead of starting cold. Add `--yolo` to let the agent write files and use tools without per-action prompts.

---

### `steer` — inject one prompt without going interactive

> *"One instruction into the existing session, no chat window."*

```bash
miaco steer --pde <uuid> -p "Implement the next action step" --yolo
miaco steer --pde <uuid> -p "<prompt>" --json     # machine-readable result
```

**You get back** the engine's response to a single non-interactive prompt, run inside the PDE's existing session and saved as an artifact. Ideal for scripts and one-shot nudges.

---

### `stc` / `pde-to-st` — chart the tension

> *"Turn this decomposition into something I can track."*

```bash
miaco stc convert <pde>        # → a structural-tension JSONL chart
miaco stc list                 # decompositions available to convert
miaco pde-to-st run --pde <uuid>   # → the Structural Thinking Four Questions
```

**You get back** the decomposition reframed as a **Structural Tension Chart** (desired outcome ↔ current reality, with action steps) or as the Four Questions — the bridge from analysis into tracked creative advancement.

---

## The engineering toolkit

Standalone command families for the wider engineering world — useful with or without the decomposition loop.

### `chart` — structural tension charts by hand

> *"Hold the gap between what I want and what's true."*

```bash
miaco chart create --outcome "Working CLI" --reality "Scaffolding exists"
miaco chart add-step --chart <id> --title "Build commands"
miaco chart complete --step "Build commands"
miaco chart review --chart <id>      # Creator Moment of Truth
miaco chart list
```

**You get back** a living chart you advance step by step, with a review that reflects progress back as a *Creator Moment of Truth* rather than a status report.

---

### `schema` · `validate` — design and check structure

> *"Design an NCP schema, and make sure my data honors it."*

```bash
miaco schema design --name "HeroJourney" --type story
miaco schema list
miaco schema export --name hero_journey --output ./schema.json

miaco validate ncp --file ./story.ncp.json --strict
miaco validate beat --file ./beat.json
miaco validate coherence --session <id>
miaco validate types --project ./tsconfig.json
```

**You get back** schemas you can design, list, and export, plus validators that check NCP structure, story beats, narrative coherence, and TypeScript types.

---

### `trace` — observe across systems

> *"Follow one story across every tool that touched it."*

```bash
miaco trace start --session my-session --story hero-journey
miaco trace log --event BEAT_GENERATED --data '{"beat_id":"beat_001"}'
miaco trace view <traceId> --format timeline
miaco trace correlation --session my-session --story hero-journey
```

**You get back** trace sessions plus correlation headers that stitch one session/story together across the wider narrative-intelligence stack.

---

### `skill` — install the packaged agent skill

```bash
miaco skill show                 # print the packaged skill
miaco skill install              # into ./.agents/skills/miaco
miaco skill install --global     # into ~/.agents/skills/miaco
```

**You get back** miaco's own agent skill dropped into your workspace, so a coding agent knows how to drive miaco.

---

## Quick utilities

```bash
miaco status      # current engineering context at a glance
miaco check       # quick type validation on the current context
miaco examples    # copy-paste starting points
miaco --help      # every command + live environment-variable values
```

---

## 🧭 Navigate

- **[README.md](./README.md)** — the philosophy and the decomposition loop
- **[STRATEGIES.md](./STRATEGIES.md)** — how `decompose run` decomposes, and which strategy to choose
