# 🔧 miaco - Engineering World Terminal Agent

**miaco** (pronounced mee-AH-koh) is the **Engineering World CLI Agent** within the mia-code ecosystem. While `miatel` handles story world and `miawa` handles ceremony, miaco is the terminal interface for **schema design, validation, and system architecture**.

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
```

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

Based on Robert Fritz's creative process:

```
┌─────────────────────────────────────────┐
│ DESIRED OUTCOME                         │
│ What you want to CREATE (not fix)       │
│                                         │
│         ↑  STRUCTURAL TENSION  ↑        │
│                                         │
│ CURRENT REALITY                         │
│ Honest factual assessment               │
└─────────────────────────────────────────┘
```

The tension between outcome and reality drives creative advancement.

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
```

---

*miaco: Where architecture becomes observable.*
