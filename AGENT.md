# Agent Instructions for lsp

You are operating within the openOODA polyrepo. The lsp package is the
Language Server Protocol server that hands IDEs the 4 core LSP methods
over stdio JSON-RPC. Your execution must be rigorous, deeply skeptical,
and strictly bound by the repository's governance laws
(`openOODA/RULES.oot` and `openOODA/FLOOR.oot`).

## 1. Zero Trust & The Double-Run Law
- **Falsify, never confirm:** A test that always passes provides no proof.
  Hostile negative-trust tests must be run TWICE in fresh processes and
  produce identical results to be considered verified.
- **LSP is editor-facing, but agents use it too.** Treat the stdio socket
  like any other capability surface: refuse unknown methods, refuse missing
  `oodar_root` cap grant, refuse `textDocument/definition` for symbols not
  in the AST.
- **25 hostile probes in qa/** exist to keep you honest. Run them on every
  change.

## 2. Services for Speed (No Shortcuts)
- **Do not blindly `grep` the tree.** Use the LSP methods directly.
- **You MUST use the native LSP method surface (4 methods):**
  - `initialize` — negotiate the cap grant from
    `${oodar_root}/sec/cap/cap_table.json`.
  - `textDocument/definition` — jump-to-definition over the AST.
  - `textDocument/references` — find-all-references over the AST.
  - `shutdown` — graceful exit (per LSP spec, mandatory before `exit`).
- The full list is in `methods/ANCHOR.oo`. There is no `textDocument/
  hover` (yet); the file format does not store doc comments.

## 3. Strict Repository Compliance
- **Pure Files:** Only `.oo` and `.oot` files are permitted for logic
  (RULES.oot §1.14). No VERSION file. Engine version is
  `lsp_facade_version()` in `server/lsp_version.oo`.
- **Line Limits:** Absolute maximum of 256 lines per file.
- **Academy Headers:** All `.oo` files must begin with the exact 4-element
  Academy header.
- **Six Functional Layers:** `wire/`, `types/`, `server/`, `methods/`,
  `symbols/`, `bridge/`. Plus `cli/`, `qa/`, `docs/`, `examples/`.
  ANCHOR.oo is the front door.
- **Pass `--oodar-root`** to `cli/` to load the cap table. No hard-coded
  cap table.

## 4. Commit Hygiene
- **One Repo, One Commit:** Never bundle changes across multiple
  repositories in a single commit.
- **Docs in the Same Commit:** Any behavioral change must be accompanied
  by the corresponding `docs/` update in the very same commit.
- **Tag = VERSION:** This repo has no VERSION file. The tag IS the version
  (RFC-0006). Bump the tag when a method is added.
