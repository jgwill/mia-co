# mia-co

Lightweight public landing page for the published `mia-co` npm package.

`mia-co` installs the `miaco` CLI: an Engineering World terminal agent for prompt decomposition, structural tension charts, schema/validation helpers, tracing, and QMD-backed inquiry workflows.

## Install

```bash
npm install -g mia-co
miaco --help
```

## Quick Use

```bash
miaco status
miaco examples
miaco decompose run -p "Map this prompt into actionable work"
miaco continue --pde <uuid>
miaco steer --pde <uuid> -p "Refine the next step"
miaco qmd search "structural tension"
```

## Main Commands

- `decompose`: create a PDE prompt decomposition with Copilot, Claude, Gemini, Codex, PVA, or Hermes.
- `continue`: resume a PDE tree and optionally run follow-up inquiry steps.
- `steer`: send a new prompt into an existing PDE session.
- `pde-to-st` / `stc`: turn decompositions into structural thinking artifacts.
- `qmd` / `qmd-inquiry-decompose`: query or enrich with QMD knowledge providers.
- `schema`, `validate`, `trace`, `chart`, `skill`: supporting engineering-world operations.

## Package Notes

The published package name is `mia-co`; the installed binary is `miaco`.

Current package metadata from the development workspace:

- Version: `0.11.1`
- License: `MIT`
- Repository: `https://github.com/jgwill/mia-co`
- Development source: `/src/mia-code/miaco`

## Development

```bash
cd /src/mia-code/miaco
npm install
npm run build
npm start -- --help
```

This repository exists as the public package landing point referenced by npm metadata.
