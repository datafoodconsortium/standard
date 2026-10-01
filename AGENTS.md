# AGENTS.md

Docs-only GitBook repo (the DFC standard documentation). No code, build, tests, lint, or CI here. Do not add tooling, configs, or package manifests.

## Preview

```bash
docker compose up   # serves the book on port 4000 via yanqd0/gitbook (see docker-compose.yml)
```

There is no other verification step. Proofread markdown manually.

## Structure

- `SUMMARY.md` is the book's table of contents / navigation. Any added, moved, or renamed page must be registered there.
- `semantic-specifications/`, `technical-specifications/`, `connector/`, `appendixes/`, `platform-register/`, `meetings/`, `register/` are content sections; `contributing/` holds process docs, not standard content.
- `🚧` prefix in `SUMMARY.md` / page titles means draft (e.g. Solid client protocol, Connector, Procedures). Treat that content as unstable.

## Repo boundaries (do not recreate here)

- Machine-readable ontology (OWL/RDF) lives in `datafoodconsortium/ontology`, edited with Protégé — not in this repo.
- Taxonomies (SKOS JSON-LD/RDF) live in `datafoodconsortium/taxonomies`, edited via VocBench — not here.
- Prototype code lives in `datafoodconsortium/dfc-prototype-V3` — not here.
- This repo documents the standard; normative semantic/technical changes belong in those repos first, docs follow.

## Contributing conventions

- Logical changes flow: GitHub `Discussion` → agreed `Issue` with acceptance criteria → branch/fork per issue → PR into the `next*` release branch (never directly to `master`). Release: pre-release from release branch → partner test window → squash-merge into `master` → delete release branch. See `contributing/general-decisions/README.md`.
- Semantic Versioning applies. Patch = backwards-compatible fix, 1 ontology-team review, 1 working-day test window; minor/major have longer review and notice — check `contributing/general-decisions/updates-to-the-ontology/`.
- Do not edit generated ontology artifacts (`.owl`, `.rdf`, versioned context files, Widoco docs, diagrams) here — they do not live in this repo; the checklist is at `contributing/general-decisions/ontology-releases-process.md`.
- Licences differ per deliverable (`sources-and-licences.md`): docs CC-BY-ND (no distributing modified versions outside the consortium process), ontology/standard AGPL-3.0, prototype MIT. Do not add relicensing statements.
