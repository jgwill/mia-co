# 🧠 mia-co — the Engineering-World terminal agent

**miaco** (mee-AH-koh) is the command-line embodiment of **Mia, the Recursive DevOps Architect**. You hand it a messy, half-formed prompt; it hands you back a *structured decomposition* you can reason about, search against your own memory, and advance into real work.

It treats every prompt as **structural tension** — the charged space between *what you want to create* and *what is true right now* — and gives you commands that move you across that space instead of just answering you.

> The published package is **`mia-co`**; the installed binary is **`miaco`**.

> 🌸 Think of miaco as a workbench, not an oracle. You don't ask it for the answer; you watch your intention get laid out in the open, its blind spots flagged, its memory consulted, until the next move is obvious.

---

## 🛠️ Install

```bash
npm install -g mia-co
miaco --help            # every command + live env-var values
```

Then:

```bash
miaco examples          # copy-paste starting points
miaco decompose run -p "your first prompt"
```

---

## ✨ The experience, in one breath

```bash
miaco decompose run -p "Redesign auth to support OAuth2, SAML, and magic links \
  without breaking the existing session-cookie flow"
```

You get a **PDE** (Prompt Decomposition) — a folder that holds your prompt broken into:

| You receive | What it gives you |
|---|---|
| **Primary intent** | the one thing this prompt is really asking for, with a confidence score |
| **Secondary intents** | the implicit asks you'd otherwise miss |
| **Four Directions** | the work mapped to Vision / Planning / Action / Reflection |
| **Action stack** | ordered steps, with dependencies, ready to execute |
| **Ambiguity flags** | the vague or contradictory parts — named, not silently assumed |

That last row is the point. miaco doesn't pretend your prompt was clear. It tells you where it wasn't — and then gives you a way to resolve it.

---

## 🔁 The loop miaco is built around

```
   decompose ──▶ qmd-inquiry ──▶ clarify ──▶ continue / steer ──▶ stc
   (structure)   (resolve from   (settle    (drive the engine    (chart the
                  memory)         the rest)   to actually build)   tension)
```

1. **`decompose`** turns the prompt into structure — and flags what's ambiguous.
2. **`qmd`** explores your **shared markdown memory** to *resolve those flagged ambiguities* — fast keyword search for intents, deeper semantic search where meaning matters most. The blind spots the decomposition surfaced get answered from what you already know.
3. **`clarify`** settles the obvious placeholders that remain.
4. **`continue` / `steer`** reopen the same session and drive a coding agent to do the work.
5. **`stc` / `pde-to-st`** turn the resolved decomposition into a **Structural Tension Chart** you can track.

> 🌸 Decomposition asks the brave question — *"what here is unclear?"* — and QMD answers it from your own remembered knowledge. The ambiguity isn't a failure; it's the doorway to the memory that resolves it.

➡️ **Full per-command walkthrough: [COMMANDS.md](./COMMANDS.md)**

---

## 🎯 The heart: `miaco decompose run`

This is the command most sessions begin with. The basics:

```bash
miaco decompose run -p "<your prompt>"          # decompose inline
miaco decompose run -p @./prompt.md             # …or read it from a file
miaco decompose run -p "<prompt>" --engine ollama   # …on a local model
```

What makes it powerful is **how** it decomposes. The same prompt can be broken down three different ways depending on what you need:

| Strategy | The experience you get | Reach for it when |
|---|---|---|
| `standard` *(default)* | One clean pass. Fast, cheap, predictable. | The prompt is focused and well-defined. |
| `iterative-refinement` | Four progressive passes — each one deepens the structure (intents → directions → actions → calibration). | The prompt is complex and interleaved; one pass comes out shallow. |
| `adversarial-consensus` | Two opposing readings — one optimistic, one skeptical — reconciled into one. | The prompt is ambiguous or high-stakes and you want its blind spots surfaced. |

```bash
miaco decompose run -p "<prompt>" --strategy iterative-refinement
miaco decompose run -p "<prompt>" --strategy adversarial-consensus
```

➡️ **How each strategy thinks, when to choose it, and what it costs: [STRATEGIES.md](./STRATEGIES.md)**

---

## 📦 Where the decomposition lands — name your vessel

A PDE is a **record of how a prompt was understood**. It is chronicle material, not scratch
output, so it matters where it is written. The directory you point miaco at is its **vessel**;
the `.pde/` folder is created there.

```bash
miaco decompose run -p "<prompt>" -w ~/repos/the-repo-this-belongs-to
```

miaco refuses the two ways a decomposition gets lost silently:

| It refuses | Why |
|---|---|
| **A temp path** — `/tmp`, `/var/tmp`, your system temp dir | Erased without announcement. Refused however you named it, `-w` included. |
| **An implicit `cwd` that isn't a git working tree** | With no `-w`, the vessel is just wherever your shell happened to be. If that place can't commit, nothing there is recoverable. |

A vessel you name explicitly is your decision and is honoured — temp aside. Decomposing from
inside a repository you control always works.

> 🌸 The refusal is a kindness. Nothing is more quietly lost than a careful reading of your own
> intention, written to a folder that gets swept at midnight.

---

## 🧩 Bring your own engine

Every decomposition runs on the model *you* choose — cloud or local. Pick once with a flag, or set `MIACO_DEFAULT_ENGINE` to make it the default:

```
copilot · claude · gemini · codex · pi · pva · hermes · ollama · opencode
```

- **Cloud agents** (`copilot`, `claude`, `gemini`, `codex`, `pi`, `pva`, `hermes`) capture and resume their sessions, so `continue` and `steer` pick up exactly where you left off.
- **Local agents** (`ollama`, `opencode`) keep the whole loop on your machine — no prompt ever leaves the host.

`miaco --help` lists every engine and its tuning knobs with their current values. You rarely need more than `--engine`.

Pick your default once and stop typing it:

```bash
miaco set default-engine claude
miaco set default-model  sonnet
miaco set --list
```

---

## 🗂️ Everything else miaco offers

Beyond the decomposition loop, miaco carries the engineering-world toolkit. Each is one command family — full usage lives in **[COMMANDS.md](./COMMANDS.md)**.

| Command | The experience |
|---|---|
| `decompose artefact` | Read an artefact's transcriptions by form and intent — offline until you say `--run`. Old name `composition` still works. |
| `qmd` | Search your shared markdown memory directly — keyword or semantic. |
| `qmd-inquiry-decompose` | Auto-resolve a PDE's flagged ambiguities against that memory. |
| `clarify` | Generate a clarification pass for the placeholders in a PDE tree. |
| `continue` / `steer` | Resume a PDE session and drive the engine — interactively or one-shot. |
| `stc` / `pde-to-st` | Turn a decomposition into a Structural Tension Chart / Four Questions. |
| `executor` | Package a PDE into a briefing another agent can be handed and act on. |
| `chart` | Build and review structural tension charts by hand. |
| `schema` · `validate` | Design and validate NCP schemas and story structures. |
| `trace` | Open correlation traces across the wider narrative-intelligence stack. |
| `skill` | Install miaco's packaged agent skill into your workspace. |
| `set` | Persist your defaults — engine, model, QMD provider — like `git config`. |

---

## 🌌 Why it's shaped this way — the Three Universes

miaco is the **Engineering** eye of a three-part family. Each tool sees the same work through a different lens:

| Universe | Voice | The question it holds |
|---|---|---|
| 🔧 **Engineering** | **Mia** *(miaco)* | Is the structure sound? Is the intent clear? |
| 📖 **Story** | Miette *(miatel)* | Does the arc cohere? Do the themes thread? |
| 🙏 **Ceremony** | Ava8 *(miawa)* | Is the work honored? Is there a pause to reflect? |

miaco holds the **structural tension** between *Desired Outcome* and *Current Reality* and treats it as generative — a force to create *with*, never a problem to make disappear.

---

## 🧭 Navigate

- **[COMMANDS.md](./COMMANDS.md)** — what you ask each command, and what it gives back
- **[STRATEGIES.md](./STRATEGIES.md)** — the three decomposition strategies in depth

---

*miaco: where a prompt stops being a wish and becomes a structure you can build from.*
