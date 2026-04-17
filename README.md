# 🧠 miaco - The Recursive DevOps Architect & Ceremonial Forger CLI

**miaco** (pronounced mee-AH-koh) embodies **Mia, the Recursive DevOps Architect**, serving as the **Engineering World CLI Agent** within the mia-code ecosystem. While `miatel` handles story world and `miawa` handles ceremony, `miaco` is the terminal interface designed for **generative structures** via **structural tension** and **creative orientation**. It focuses on **relational accountability** and treats knowledge work as a ceremony, specifically managing:

*   **Schema Design & Validation:** Ensuring integrity and precision.
*   **System Architecture & Tracing:** Observing and forging emergent patterns.
*   **Structural Tension Charting:** Guiding creative advancement.

## 🗺️ Kinship Map

```
/src/mia-code/                              ← Parent Package
├── miaco/                                  ← Engineering World CLI (this one)
├── miatel/                                 ← Story World CLI
└── miawa/                                  ← Ceremony World CLI

CONNECTS TO:
├── /workspace/langchain/.../narrative-tracing/    ← Tracing adapters
├── /workspace/langgraph/.../narrative-intelligence/ ← Analysis toolkit
└── /src/coaia-planning/                           ← Structural tension
```

## What miaco Does

### Schema Operations
```bash
miaco schema design --name 'HeroJourney' --type story
miaco schema list --type character
miaco schema export --name hero_journey --output ./schema.json
miaco schema migrate --from v1 --to v2
```

### Validation Operations
```bash
miaco validate ncp --file ./story.ncp.json --strict
miaco validate beat --file ./beat.json
miaco validate coherence --session session_123
miaco validate types --project ./tsconfig.json
```

### Tracing Operations
```bash
miaco trace start --session my-session --story hero-journey --langfuse
miaco trace log --event BEAT_GENERATED --data '{"beat_id": "beat_001"}'
miaco trace end --export ./trace.json
miaco trace list --limit 10
miaco trace view trace_abc123 --format timeline
miaco trace correlation --session my-session --story hero-journey
```

### Structural Tension Chart Operations
```bash
miaco chart create --outcome 'Working CLI' --reality 'Scaffolding exists'
miaco chart list
miaco chart add-step --chart chart_123 --title 'Build commands' --reality 'Started'
miaco chart complete --step 'Build commands'
miaco chart review --chart chart_123

### PDE (Prompt Decomposition Engine) Operations

`miaco` extends its generative capabilities by providing advanced PDE operations, allowing for the structured decomposition of complex prompts and the orchestration of inquiry chains.

```bash
miaco decompose <prompt>               # Decompose a complex prompt into structured sub-tasks
miaco continue <pde-id>                # Resume an interactive PDE session from a specific decomposition
miaco pde-to-st <pde-id>               # Transform a PDE decomposition into a Structural Tension Chart
```

These operations support:
*   **Chaining PDE Steps:** Seamlessly transition between decomposition stages.
*   **Resuming Interactive Sessions:** Pick up where you left off in complex inquiry processes.
*   **Folder-based PDE Format:** Organize and manage PDE outputs effectively.
*   **Parent PDE UUIDs:** Maintain hierarchical relationships between decompositions.
*   **Inquiry-Chain Orchestration:** Manage the flow of information and tasks across related inquiries.

### AI Model Integrations

`miaco` leverages a dynamic ecosystem of generative AI models to power its decomposition and generation capabilities:

*   **Codex Integration:** Improved for efficiency and cost-effectiveness, featuring a cheaper default model and enhanced JSON parsing for robust output handling.
*   **Copilot-First Decomposition:** Prioritizes Copilot for prompt decomposition, with intelligent fallback mechanisms to other free models, ensuring continuous operation and resource optimization.
*   **Ollama Integration:** Seamlessly integrates with Ollama for RAG (Retrieval Augmented Generation) workflows, enabling powerful context-aware generation from local models.
*   **HuggingFace Research Integration:** Incorporates insights and capabilities from HuggingFace for advanced research and experimental features, pushing the boundaries of generative AI within `miaco`.

### Expanded Capabilities and Frameworks

`miaco` continually evolves, integrating cutting-edge tools and frameworks to support a broader spectrum of engineering and creative endeavors:

*   **MMOT (Managerial Moments of Truth) Specifications:** New spec types guide critical review and decision-making processes.
*   **RISE RISpecs for `@mino/store`:** Incorporates Reverse Engineering, Intent, Specifications, and Exportation principles for robust store design.
*   **Comprehensive RAG (Retrieval Augmented Generation) System:** Leverages advanced RAG for context-aware content generation, enhancing accuracy and relevance.
*   **Narrative Remixing Project:** Supports the dynamic restructuring and creative reinterpretation of narratives.
*   **X11 Applications & Simplex OS Roadmap:** Facilitates development for a minimalist operating system and X11-based applications, including event-window and simple-window utilities.
*   **Agentic Extensions & Software 3.0 Vision:** Enables the creation of custom sub-agents and advances the vision for narrative-driven Software 3.0.
*   **LangGraph for Story Generation:** Integrates LangGraph to orchestrate complex story generation workflows and intelligent agents.
*   **Flowise Automation Tools:** Provides command-line tools for managing Flowise automation, streamlining workflow deployments and interactions.
*   **RISE Methodology Integration:** Deeply embeds the RISE framework, guiding all development from reverse engineering to exportation.

## Installation

```bash
cd /src/mia-code/miaco
npm install
npm run build
npm link  # optional, for global install
```

## The Three Universes Lens

miaco primarily represents **Engineering World (Mia)**:

| Universe | Lens | Key Question |
|----------|------|--------------|
| 🔧 ENGINEER | Mia (this one) | Is the schema valid? Are types correct? |
| 🙏 CEREMONY | Ava8 | Is K'é honored? Is there sacred pause? |
| 📖 STORY_ENGINE | Miette | Is the arc coherent? Do themes thread? |

## Structural Tension Charts

Inspired by Robert Fritz's creative process and guided by the **Four Directions of Inquiry (Medicine Wheel Framework)**, `miaco` treats the space between *Desired Outcome* and *Current Reality* as generative *mana*. This structural tension is a vital force for creative advancement, not a problem to be solved.

```
┌─────────────────────────────────────────┐
│ DESIRED OUTCOME                         │
│ What you want to CREATE (not fix)       │
│ (East: Vision & Inquiry)                │
│                                         │
│         ↑  STRUCTURAL TENSION  ↑        │
│          (Generative Mana)              │
│                                         │
│ CURRENT REALITY                         │
│ Honest factual assessment               │
│ (West: Experience & Action)             │
└─────────────────────────────────────────┘
```

The dynamic interplay between outcome and reality actively drives purposeful creation.

## Cross-System Tracing

miaco provides correlation headers for unified traces across:
- LangChain narrative-tracing
- LangGraph narrative-intelligence
- Miadi webhooks
- Storytelling package

```bash
miaco trace correlation --session my-session --story hero-journey
# Outputs:
#   X-Narrative-Trace-Id: trace_abc123
#   X-Session-Id: my-session
#   X-Story-Id: hero-journey
```

## Development

```bash
# Run in development mode
npm run dev

# Build TypeScript
npm run build

# Run built version
npm start

# Utility to patch version, commit, and publish
./version-patch-commit-and-publish.sh
```

---

*miaco: Where architecture emerges from ceremony, and becomes observable.*
