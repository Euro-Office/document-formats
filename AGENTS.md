# document-formats — Euro-Office Format Registry

@../AGENTS.md

Guidance for Claude Code (and other AI agents) working in **document-formats** — the canonical document-format definition.

## What this repo is
A single JSON data file, `onlyoffice-docs-formats.json`, defining every document format the suite supports, plus `build.js` which regenerates `README.md` from it. There is no runtime library — the JSON is read directly by consumers via file path.

## Schema & Consumers
- **Entry shape:** each entry has exactly `name`, `type` (`word`/`cell`/`slide`/`pdf`/`diagram`), `actions` (e.g. `view`, `edit`, `auto-convert`, `fill`, …), `convert` (array of target format names), and `mime` (array of MIME strings). There are no boolean routing flags — conversion targets are listed by name in `convert`.
- **`name` is the cross-repo identifier.** It is used as the file-extension key by `server`, `web-apps`, and the `core` converter. **CRITICAL:** renaming or removing a `name` silently breaks every consumer that hardcodes it — grep all repos before touching one.
- **`build.js` only regenerates the README.** It reads the JSON and rewrites the "Supported formats" tables in `README.md`. It is **not** a schema validator or a build gate — there is no structural validation step in this repo.

## Rules
- **Never** edit `README.md` by hand — run `node build.js` after any JSON change so the tables stay in sync.
- **Never** change a MIME type or extension without cross-referencing the file-upload handling in `server/DocService`.
- **Never** add an entry without all five fields, and verify its `type` is an existing editor type (`word`, `cell`, `slide`, `pdf`, `diagram`).

## Findings & Long-tail
No centralized findings store exists in this repository yet. Document edge cases in code comments or GitHub issues until one is established.
