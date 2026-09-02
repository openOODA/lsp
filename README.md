# openOODA/lsp — openOODA Native LSP Server

The openOODA native LSP engine is a pure-`.oo` self-hosted language server. It speaks LSP 3.17 (JSON-RPC 2.0 over stdio) and provides hover, completion, definition, diagnostics, formatting, code actions, and document symbols — all in-process, no third-party LSP library required.

## Part of the openOODA ecosystem

This repo is one of six in the [openOODA](https://github.com/openOODA) polyrepo:

- [openOODA/openOODA](https://github.com/openOODA/openOODA) — governance, process boards, RFCs
- [openOODA/ooda](https://github.com/openOODA/ooda) — the compiler, runtime, standard library
- [openOODA/opm](https://github.com/openOODA/opm) — the OODA package manager library
- **openOODA/lsp** — this repo, the LSP server
- [openOODA/.github](https://github.com/openOODA/.github) — shared GitHub meta
- [openOODA/openOODA.github.io](https://github.com/openOODA/openOODA.github.io) — website

## Quick start

```sh
# Build
ooda run ooda/lsp/scripts/build.oo

# Test (runs the 8 probe drivers)
ooda run ooda/lsp/scripts/test.oo

# Verify VERSION + api_surface
ooda run ooda/lsp/scripts/verify.oo

# Run the LSP server (stdio)
ooda lsp

# Run as one-shot file mode
ooda lsp path/to/request.txt
```

## Repo layout

```
lsp/
  ANCHOR.oo          front door; Academy header + import registration
  VERSION            compat contract (lsp version, ooda range, api_surface)
  wire/              JSON-RPC, stdio, encoding, method classification
  types/             wire type system (LspFrame, LspDoc, LspDiagnostic, LspCodeAction)
  server/            server loop, dispatch, guards, handlers, capabilities
  methods/           hover, completion, definition, formatting, codeAction, diagnostics
  symbols/           canonical symbol index, stdlib scan, E-M telemetry
  bridge/            compiler integration (uses &CompilerReadCap)
  qa/                self-tests (8 probe drivers)
  scripts/           build, test, verify, release
  docs/              LSP_SETUP.oot, protocol.oot, client_config.oot
  examples/          sample JSON-RPC request/response pairs
```

Every dir has an `ANCHOR.oo` at the top (per the openOODA tactical-subsystem template). The pattern is uniform across the project.

## Capabilities

| Method | Purpose |
|---|---|
| `initialize` | Lifecycle start, server capabilities |
| `textDocument/completion` | Triggers on `.`, `&`, `:`; canonical stdlib seed + user module index |
| `textDocument/hover` | Markdown with the Academy 4-element header prepended |
| `textDocument/definition` | URI + line + col for `pub fn` and capability builtins |
| `textDocument/documentSymbol` | `pub fn` symbols, kind=Function |
| `textDocument/formatting` | Normalized indentation, single full-file TextEdit |
| `textDocument/codeAction` | `E_CAP` → add `&Cap`, `E_PARSE` → close brace, `E_EM_WEIGHT` → split file, `E_CAP_ATTENUATE` → narrow cap |
| `textDocument/diagnostic` | E-M gauge readings → LSP Diagnostic JSON |
| `workspace/executeCommand` | `ooda.lsp.version`, `ooda.lsp.indexSize`, `ooda.lsp.echo` |

See `docs/LSP_SETUP.oot` for the full setup guide and `docs/protocol.oot` for the wire-protocol details.

## Compatibility

`lsp` requires `ooda >= 2.10.0`. See `VERSION` for the full contract. The `api_surface` field is a structural check: the loader asserts the mod.oo exposes exactly 6 public subdirs.

## License

Dual-licensed under Apache 2.0 or MIT, at your option. See `LICENSE`.
