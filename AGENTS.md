# lsp: Agent Engineering Standards (v1)

This repository houses the Language Server Protocol (LSP) daemon for openOODA.
All work in this repository strictly defers to the organization standards in [`openOODA/AGENTS.md`](file:///home/ubermetroid/Projects/openOODA/openOODA/AGENTS.md).

---

## 1. Language Server Architecture & Invariants
- **JSON-RPC 2.0 Stdio**: Editor diagnostics, hover, go-to-definition, document outline, and rename.
- **Incremental Ast Caching**: Efficient AST digests without re-parsing unchanged files.
- **Capability Visibility**: Real-time inspection of inferred and required capability tokens.

---

## 2. Invariants & Quality Standards
- **The Page Rule**: Every `.oo` page must be between 16 and 256 lines.
- **Directory Density**: At most 8 `.oo` pages per directory.
- **4-Element Academy Header**: Mandatory on every `.oo` page.
- **Double-Run Determinism**: All `qa/probe_*.oo` verification probes must pass in sequential fresh processes.

---

## 3. Local Verification Commands
```bash
cli build cli/main.oo -o dist/lsp
cli qa
```
