<!--
  ██╗  ██╗██╗   ██╗██████╗ ██████╗  █████╗ ███╗   ██╗███████╗████████╗
  ██║  ██║╚██╗ ██╔╝██╔══██╗██╔══██╗██╔══██╗████╗  ██║██╔════╝╚══██╔══╝
  ███████║ ╚████╔╝ ██║  ██║██████╔╝███████║██╔██╗ ██║█████╗     ██║
  ██╔══██║  ╚██╔╝  ██║  ██║██╔══██╗██╔══██║██║╚██╗██║██╔══╝     ██║
  ██║  ██║   ██║   ██████╔╝██║  ██║██║  ██║██║ ╚████║███████╗   ██║
  ╚═╝  ╚═╝   ╚═╝   ╚═════╝ ╚═╝  ╚═╝╚═╝  ╚═╝╚═╝  ╚═══╝╚══════╝   ╚═╝
                                    ʊ-Net
-->

<div align="center">

```
██╗ ██╗    ███╗   ██╗███████╗████████╗
╚██╔╝╚██╗  ████╗  ██║██╔════╝╚══██╔══╝
 ╚═╗  ╚═╗  ██╔██╗ ██║█████╗     ██║
 ██╔╝  ██╔╝ ██║╚██╗██║██╔══╝     ██║
██╔╝  ██╔╝  ██║ ╚████║███████╗   ██║
╚═╝   ╚═╝   ╚═╝  ╚═══╝╚══════╝   ╚═╝
        HydraNet  ·  ʊ-Net
```

**The Self-Healing Distributed Multi-Agent Engineering Swarm**

*When an agent gets stuck, it doesn't retry harder — it rethinks from scratch.*

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Pre--Alpha-orange)](/)
[![Stack](https://img.shields.io/badge/Stack-Python_%7C_TypeScript-green)](/)
[![Nodes](https://img.shields.io/badge/Cluster-3%20Nodes_%7C_22GB%20VRAM-purple)](/)
[![Built In Public](https://img.shields.io/badge/Built-In%20Public-ff69b4)](/)

> *"Three gaming laptops. Zero cloud. A swarm of self-healing agents that quit when they should and retry when they must."*

</div>

---

## Why this exists

Every AI coding agent in production today shares a fatal flaw called **The Death Spiral**.

```
 Error on line 47
      │
      ▼
 Agent tweaks line 47 slightly
      │
      ▼
 Same error on line 47
      │
      ▼
 Agent tweaks line 47 again (differently than before, but still just line 47)
      │
      ▼
 Same error on line 47
      │
      ▼
 [repeats 23 more times until context window explodes or token budget dies]
```

A human engineer reads that error, realizes the bug is actually three files upstream in the schema definition, opens that file, and fixes it there. The agent doesn't. It can't. It doesn't know how to **quit a dead approach and try a different one**.

HydraNet (ʊ-Net) is built to solve exactly that: detect when an agent has entered a death spiral, activate what we call **The Reaper Protocol** — kill the branch, roll the workspace back, inject the failure history, and force a genuinely different strategy.

Everything else in this repo — the AST-aware graph retrieval, the three-node mesh, the live genealogy dashboard — exists to support that one core idea.

---

## The Architecture

```
                         [ Bug report / GitHub issue ]
                                      │
                                      ▼
  ╔═══════════════════════════════════════════════════════════════════╗
  ║         NODE A  ─  THE SUPERVISOR  (M. Yadav · RTX 3050 · 6GB)   ║
  ║                                                                   ║
  ║  ┌─────────────────┐   ┌──────────────┐   ┌──────────────────┐   ║
  ║  │  State Machine  │──▶│ Convergence  │──▶│ Reaper Protocol  │   ║
  ║  │  (LangGraph)    │   │  Detector    │   │ (rollback engine)│   ║
  ║  └─────────────────┘   └──────────────┘   └──────────────────┘   ║
  ║          │                                         │              ║
  ║   routes tasks                             fires on threshold     ║
  ╚══════════╪═════════════════════════════════════════╪═════════════╝
             │                                         │
      ┌──────┴──────┐                         ┌────────┴───────┐
      ▼             ▼                         ▼                ▼
  [Code tasks] [Review tasks]           [Rollback workspace]
      │                                  [Rebranch + re-plan]
      │
  ╔═══╧═══════════════════════════╗   ╔═══════════════════════════════╗
  ║ NODE B  ─  INFERENCE WORKER   ║   ║ NODE C  ─  SANDBOX WORKER     ║
  ║ (Y. Jangra · RTX 4060 · 8GB) ║   ║ (B. Kataria · RTX 4060 · 8GB)║
  ║                               ║   ║                               ║
  ║  Qwen2.5-Coder-7B (4-bit AWQ) ║   ║  Docker ephemeral sandbox     ║
  ║  BAAI/bge embedding server    ║   ║  pytest / jest executor       ║
  ║  Tree-sitter AST parser       ║   ║  git checkpoint engine        ║
  ║  pgvector graph retrieval     ║   ║  diff calculator              ║
  ╚═══════════════════════════════╝   ╚═══════════════════════════════╝
             │                                         │
             │◀────────── gRPC / WebSocket ───────────▶│
             │             (LAN · TLS)                  │
             │                                         │
             └─────────────────┬───────────────────────┘
                               ▼
                   ╔═══════════════════════╗
                   ║   LIVE DASHBOARD      ║
                   ║   (Next.js + WS)      ║
                   ║                       ║
                   ║  • Agent thought tree ║
                   ║  • Genealogy view     ║
                   ║  • Node VRAM meters   ║
                   ║  • War Room mode      ║
                   ╚═══════════════════════╝
```

Each node runs its **own independent model** doing its **own specialist job**. They coordinate over your local WiFi. This is a distributed multi-agent mesh — not tensor-parallel model splitting across machines (that's a different project with different tradeoffs).

---

## The Reaper Protocol — how the death spiral gets killed

This is the feature that doesn't exist anywhere else.

```
ATTEMPT  │  CODE CHANGE              │  CONVERGENCE SCORE  │  OUTCOME
─────────┼───────────────────────────┼─────────────────────┼──────────────
    1    │  Patch auth.py line 47    │  0.12               │  Tests fail
    2    │  Patch auth.py line 47-48 │  0.51  ▲            │  Tests fail
    3    │  Patch auth.py line 46-49 │  0.82  ▲▲           │  Tests fail
    3.1  │  ── REAPER FIRES ──       │  > 0.75 threshold   │
         │  git branch --move        │                     │
         │  inject failure context   │                     │
         │  force new strategy       │                     │
    4    │  Trace call to db.ts      │  0.08  ▼▼▼          │  Tests fail
    5    │  Fix schema in users.sql  │  0.19               │  PASS ✓
```

**Convergence Score** is a metric we compute per-attempt:

```
  convergence_score = α · patch_similarity(attempt_n, attempt_n-1)
                    + β · command_repetition_rate(last_k_attempts)
                    + γ · error_signature_match(stderr_n, stderr_n-1)

  threshold = 0.75   (configurable in hydra.config.json)
  k = 3              (lookback window)
```

When the score crosses the threshold, the Reaper fires regardless of how "close" the agent thinks it is. Close doesn't matter. A dead approach is a dead approach.

---

## The Genealogy View — what every agent forgets to track

The live dashboard renders a **decision tree** of every approach attempted, not just the final one:

```
  ROOT: "Fix JWT expiry bug in auth middleware"
  │
  ├── [BRANCH A · DEAD] Patch token validation in auth.ts
  │     ├── attempt 1 → TypeError ✗
  │     ├── attempt 2 → TypeError (same) ✗
  │     └── attempt 3 → ── REAPER ── convergence=0.81
  │
  ├── [BRANCH B · DEAD] Update token expiry constant
  │     ├── attempt 1 → Tests fail (different error) ✗
  │     └── attempt 2 → ── REAPER ── convergence=0.79
  │
  └── [BRANCH C · ACTIVE] Investigate refresh token handler in middleware.ts
        ├── attempt 1 → SyntaxError ✗
        └── attempt 2 → ✓ PASSING (12/12 tests)
```

No current open-source agent surfaces this. They either show you the current state or the final result. HydraNet keeps the whole exploration history alive so you can audit exactly why the agent changed course — and why *you* should trust the answer it landed on.

---

## AST-aware retrieval vs. flat embeddings — the problem in one diagram

```
  FLAT EMBEDDING SEARCH:
  ─────────────────────
  Error: "Cannot read property 'id' of undefined in auth.ts:47"

  Query → [ auth token validation ]
  Match → [ auth.ts ]   ✓ (correct)
  Miss  → [ db.ts   ]   ✗ (root cause is here — but semantic distance is too high)
  Miss  → [ schema/users.sql ]  ✗ (even further semantically)

  Result: Agent sees auth.ts. Auth.ts looks fine. Agent edits auth.ts.
          Same error. Repeat.


  AST GRAPH WALK:
  ─────────────────────
  auth.ts:47 → imports resolveUser() from db.ts
  db.ts:22   → queries User.findById() → mutates users schema
  users.sql  → id column is nullable (this is the bug)

  Result: Agent sees auth.ts → db.ts → users.sql.
          Fixes the nullable id column in the schema.
          Tests pass. Done.
```

HydraNet parses every file with **Tree-sitter** into a real AST, extracts the import/call/inheritance graph, and stores it in **pgvector**. Retrieval walks the graph, not just the similarity space. Dead bugs that live three hops from the error site get found.

---

## Team

| Node | Person | Title | Owns | Hardware |
|------|--------|-------|------|----------|
| **A** | **M. Yadav** | Project Lead · Systems Architect | State machine, Reaper Protocol, networking, orchestration, convergence scoring | RTX 3050 · 6GB VRAM |
| **B** | **Y. Jangra** | Core Contributor · Inference Lead | Model serving, quantization, prompting, AST parsing, code retrieval, genealogy logic | RTX 4060 · 8GB VRAM |
| **C** | **B. Kataria** | Core Contributor · Infrastructure Lead | Docker sandbox, git checkpoint engine, diff tooling, dashboard backend, CI | RTX 4060 · 8GB VRAM |

**Total cluster VRAM: 22GB · Total daily cloud token cost: $0.00**

---

## Tech Stack

| Layer | Choice | Rationale |
|-------|--------|-----------|
| Orchestration | Python 3.11 + LangGraph | Cyclic state graphs map directly onto retry/branch/rollback logic |
| Model serving | Ollama → vLLM (when stable) | Qwen2.5-Coder-7B-Instruct at 4-bit AWQ fits 4.1GB VRAM on 4060 |
| Supervisor model | Phi-3-Mini (4-bit GGUF) | Fast routing decisions at 3.1GB on 3050; no code generation needed |
| Embedding | BAAI/bge-large-en-v1.5 | 1.3GB VRAM; co-hosted on Node B alongside the coder model |
| Code parsing | Tree-sitter | Real AST — not regex, not line-count chunking |
| Graph + vector store | PostgreSQL 16 + pgvector | One DB, not two; relational schema handles import edges naturally |
| Sandbox | Docker (Moby) | Ephemeral per-attempt; only the repo slice gets mounted |
| Node transport | gRPC (Protocol Buffers) | Typed, streaming, binary — right choice over raw WebSockets for agent payloads |
| Auth | TLS + pre-shared token | Audited. Do not roll your own crypto. |
| Dashboard | Next.js 14 + shadcn/ui | Server components for log streaming; WebSocket for live state |
| Dashboard state | Zustand | Minimal; no Redux overhead for a 3-person project |
| CLI | Typer (Python) | Generates `--help` from type annotations automatically |
| CI | GitHub Actions | Lint + integration tests on every PR |
| Language targets | Python first, TypeScript phase 2 | Expand after the core works, not before |

---

## Hardware fit — real numbers

| Machine | Model loaded | VRAM used | Context headroom | Job |
|---------|-------------|-----------|-----------------|-----|
| Node A (3050 · 6GB) | Phi-3-Mini 4-bit GGUF | ~3.1GB | ~1.8GB free for KV cache | Supervisor, state tracking, routing, Reaper |
| Node B (4060 · 8GB) | Qwen2.5-Coder-7B 4-bit AWQ + bge-large | ~4.1GB + 1.3GB = 5.4GB | ~2.6GB for 16k context | Code planning, implementation, AST retrieval |
| Node C (4060 · 8GB) | No model (CPU-only tasks) | 0GB | Full 8GB available for Docker | Test execution, sandboxing, checkpointing |

Node C carries **zero model load** — its VRAM is completely available to Docker for heavy test workloads. This is intentional.

---

## Competitive positioning — honest version

| Tool | Stars (Jul 2026) | Cloud required | Loop-breaking | AST retrieval | Self-hosted LLM |
|------|-----------------|----------------|--------------|--------------|----------------|
| OpenHands | ~78.5k | No (Docker) | No | No | Partial |
| Cline | ~63k | No | No | No | Yes |
| Aider | ~46k | No | No | Tree-sitter map (read-only) | Yes |
| Devin | Closed-source | Yes (cloud-only) | Partial | Unknown | No |
| **HydraNet (ʊ-Net)** | **Building** | **No** | **Yes — Reaper Protocol** | **Yes — graph walk** | **Yes** |

We are not trying to replace any of these. Cline is excellent. Aider is excellent. We are trying to do one thing none of them do: systematically detect and break dead code-fix loops on local hardware, and surface the decision history so the developer can audit it.

---

## The Build Phases

> Six phases, three people, one feature at a time. Each phase ends with a **demo milestone** — something you can screen-record. No phase unlocks until the previous demo works.

| Phase | Name | Demo milestone | Lead owner |
|-------|------|---------------|------------|
| **Ⅰ** | Single-Node Agent | CLI fixes a real Python bug, end-to-end, on one laptop inside Docker | All three |
| **Ⅱ** | The Reaper Protocol | Agent backtracks after 3 spiral attempts — recorded side-by-side vs. baseline | M. Yadav |
| **Ⅲ** | AST Graph Retrieval | Side-by-side: flat embeddings misses the bug, graph walk finds it | Y. Jangra |
| **Ⅳ** | Three-Node Mesh | Phase Ⅰ CLI command runs across all 3 physical laptops | M. Yadav |
| **Ⅴ** | War Room Dashboard | Live UI showing genealogy tree, node meters, state stream | Y. Jangra |
| **Ⅵ** | Benchmark + Ship | Real SWE-bench Lite numbers published, failures included | B. Kataria |

---

## Phase Task Tables

> Checkboxes render in GitHub but are only click-to-toggle inside Issues/PRs — not in a flat `.md` file.  
> To mark done: change `- [ ]` → `- [x]` and commit, or mirror to a GitHub Project board.

---

### Phase Ⅰ — Single-Node Agent Loop

*Goal: one laptop, one bug, fixed end-to-end. No networking. No multi-node. Just the core loop working.*

| # | M. Yadav — Orchestration & State Machine | Y. Jangra — Inference & Prompting | B. Kataria — Sandbox & Execution |
|---|------------------------------------------|-----------------------------------|-----------------------------------|
| 01 | - [ ] Scaffold monorepo (packages: `orchestrator`, `inference`, `sandbox`, `dashboard`, `shared`) with `uv` workspaces and strict `pyproject.toml` | - [ ] Set up Ollama serving Qwen2.5-Coder-7B-Instruct (4-bit AWQ) on Node B, confirm it starts under 4.5GB VRAM | - [ ] Write the base Dockerfile: Python 3.11-slim, pinned system deps, no root, no host network |
| 02 | - [ ] Build the core 5-state machine: `PLAN → IMPLEMENT → TEST → REFLECT → DONE/ABORT` | - [ ] Benchmark tokens/sec and maximum usable context at 4-bit on 8GB — write the numbers down | - [ ] Build `SandboxManager`: create container, mount repo slice, exec commands, capture stdout/stderr separately |
| 03 | - [ ] Write the Typer CLI entrypoint: `hydra fix --repo PATH --issue "description"` | - [ ] Write `InferenceClient` wrapper: POST to Ollama, stream tokens, timeout after N seconds | - [ ] Implement command safety guard: block `rm -rf /`, `chown`, `chmod 777`, stray `curl` — log blocked attempts |
| 04 | - [ ] Build the agent tool set in Phase Ⅰ scope: `read_file`, `write_file`, `list_dir`, `run_shell` | - [ ] Write the `PLAN` prompt: given issue + repo structure, output a numbered step list as JSON | - [ ] Build `ResultParser`: structured pass/fail/error signal from pytest/jest stdout |
| 05 | - [ ] Build the state transition router: map tool outputs to next state | - [ ] Write the `IMPLEMENT` prompt: given step N from the plan and file contents, output a unified diff | - [ ] Implement container health check loop: confirm sandbox is live before routing any commands to it |
| 06 | - [ ] Build the `RunLogger`: append every state, tool call, output, and timestamp to a JSONL file | - [ ] Write the `REFLECT` prompt: given test failure output, what is the most likely root cause? | - [ ] Build volume mount controller: mount only the relevant subdirectory, not the entire host |
| 07 | - [ ] Add a hard execution budget: max N steps, max M tokens, abort with a clear log entry on breach | - [ ] Write `StructuredOutputParser`: extract JSON from model output even when the model wraps it in markdown prose | - [ ] Implement container teardown: stop, remove, prune volumes on success, failure, or crash |
| 08 | - [ ] Write the config system: `hydra.config.json` for model endpoints, timeouts, retry limits, budget | - [ ] Build the diff-apply utility: takes a unified diff string, validates it, applies it to the file | - [ ] Build environment variable injection: pass API keys and config into the container without leaking to host |
| 09 | - [ ] Write integration test: seed a known bug in a toy repo, run the CLI, assert the file changed correctly | - [ ] Write prompt version control: every prompt is a named, versioned `.md` template — no hardcoded strings | - [ ] Write integration test: mount a toy repo, run `pytest`, assert result parser returns the right structure |
| 10 | - [ ] **Phase Ⅰ demo:** record the terminal showing the CLI fixing a real bug in a real small repo | - [ ] **Phase Ⅰ demo:** document exact VRAM and latency numbers from the run | - [ ] **Phase Ⅰ demo:** record the Docker logs showing clean spin-up, execution, and teardown |

---

### Phase Ⅱ — The Reaper Protocol

*Goal: convergence scoring, loop detection, git-based rollback, forced re-plan. The core feature of the whole project.*

| # | M. Yadav — Reaper Engine & Convergence | Y. Jangra — Reflection Prompting & Analysis | B. Kataria — Workspace Checkpoints & Rollback |
|---|------------------------------------------|----------------------------------------------|-----------------------------------------------|
| 01 | - [ ] Design the `ConvergenceDetector` interface: inputs (diff history, command history, stderr history), output (score 0–1) | - [ ] Write `SelfReflectionPrompt`: given N failed attempts, produce structured analysis of why each one failed | - [ ] Build `WorkspaceCheckpointer`: git branch per attempt (`hydra/attempt-001`, etc.) — no raw `--hard` resets |
| 02 | - [ ] Implement `patch_similarity()`: normalized edit distance between the last two diffs (Levenshtein on the changed line sets) | - [ ] Write `ForcedStrategyShiftPrompt`: inject the full failure history and explicitly forbid the dead approach | - [ ] Build `RollbackEngine`: given a branch name, reset the working tree to that branch's HEAD cleanly |
| 03 | - [ ] Implement `command_repetition_rate()`: sliding window over last k shell commands, compute repetition fraction | - [ ] Write `StackTraceParser`: extract file, line, error type from Python/Node/Go stack traces into a structured dict | - [ ] Build `DiffCalculator`: fast diff between two directory snapshots without calling git (for in-flight comparison) |
| 04 | - [ ] Implement `error_signature_match()`: hash the dominant error type + file from stderr; compare across attempts | - [ ] Build `FailureMemoryStore`: an in-session store of (attempt_n, strategy, result, error_signature) injected into every subsequent prompt | - [ ] Build `BranchCleaner`: prune dead `hydra/attempt-*` branches after a successful run, keep them on failure for audit |
| 05 | - [ ] Implement the weighted `convergence_score` aggregation with configurable α, β, γ weights | - [ ] Write `HypothesisGeneratorPrompt`: given the failure tree so far, generate 3 alternative approaches — pick the highest-confidence one | - [ ] Implement `TransactionWrapper`: wrap all file writes in a "commit or rollback" unit — partial writes don't leave a corrupted state |
| 06 | - [ ] Build the `ReaperHook`: fires when convergence score > threshold, halts the current branch, emits a `REAPER_FIRED` event | - [ ] Write `ApproachRankingPrompt`: given a list of candidate strategies, score each by estimated probability of success | - [ ] Build sandbox reset on rollback: container is torn down and re-spun from the checkpoint branch state |
| 07 | - [ ] Build the `BranchManager`: creates a new git branch per approach, tracks the genealogy tree as a JSON structure | - [ ] Run a controlled A/B test: 10 seeded bugs, baseline vs. Reaper agent — measure attempts-to-resolution and token spend | - [ ] Write `CheckpointMetadata`: record what files were changed, what tests ran, what the exit code was at each checkpoint |
| 08 | - [ ] Build the `ReaperNotifier`: publish the `REAPER_FIRED` event to the WebSocket stream so the dashboard can react | - [ ] Write `ErrorClassifier`: buckets stderr into (syntax error / type error / runtime error / test assertion / unknown) for better prompt routing | - [ ] Write integration test: inject a bug that requires finding the root cause 2 files upstream — assert the Reaper fires and the final answer is correct |
| 09 | - [ ] Build a `SpiralSimulator` test harness: feed the agent a bug that has no solution at the file it's looking at — verify Reaper fires at the right threshold | - [ ] Write `PostMortemReport`: after a successful run, generate a human-readable explanation of every strategy attempted and why each was abandoned | - [ ] Implement filesystem-level cleanup between rollbacks: stale `.pyc`, `__pycache__`, `node_modules/.cache` removed automatically |
| 10 | - [ ] **Phase Ⅱ demo:** side-by-side screen recording — baseline agent looping 20+ times vs. HydraNet backtracking after 3 and solving it | - [ ] **Phase Ⅱ demo:** show the `PostMortemReport` output for the demo bug | - [ ] **Phase Ⅱ demo:** show the git log with per-attempt branches and rollback history |

---

### Phase Ⅲ — AST Graph Retrieval

*Goal: structural code search. The bug three hops from the error site gets found. Flat embeddings don't.*

| # | M. Yadav — Graph Store & Query Layer | Y. Jangra — AST Parser & Index Builder | B. Kataria — Hybrid Retrieval & Context Assembly |
|---|--------------------------------------|----------------------------------------|--------------------------------------------------|
| 01 | - [ ] Deploy pgvector on the Node A Postgres 16 instance; write the schema migration | - [ ] Wire Tree-sitter Python grammar; parse a file into a `FunctionDef`, `ClassDef`, `Import` node list | - [ ] Build `HybridSearchEngine`: cosine similarity (pgvector) + BM25 keyword match + graph-neighbor expansion |
| 02 | - [ ] Design the graph schema: `code_nodes(id, file, type, name, signature, embedding)`, `code_edges(src, dst, edge_type)` | - [ ] Extract call graph: for each function call in the AST, record which function it calls and in which file | - [ ] Implement `ParentChildExpander`: when a function chunk matches, pull in its containing class and the file's import block |
| 03 | - [ ] Build `IndexManager`: on `hydra fix`, scan the repo, parse every `.py` file, embed chunks, upsert into pgvector | - [ ] Extract import graph: for each `import` or `from ... import`, record the source-target file edge | - [ ] Build `GraphHopRetriever`: given a seed node, walk the import/call graph up to depth N and collect reachable nodes |
| 04 | - [ ] Build incremental indexing: hash each file's content; only re-embed files that changed since the last run | - [ ] Implement `ASTHasher`: stable hash per function/class body — change detection without full re-parse | - [ ] Implement `StructuralRanker`: weight nodes higher that are imported by many other files in the repo |
| 05 | - [ ] Build a `VectorCacheLayer`: LRU cache on the 100 most-recently-retrieved code chunks to avoid repeated DB round-trips | - [ ] Wire Tree-sitter TypeScript grammar; test the same parser pipeline on a `.ts` file | - [ ] Build `ArtifactFilter`: skip `node_modules/`, `venv/`, `dist/`, `*.lock`, `*.pyc`, `__pycache__/` at index time |
| 06 | - [ ] Build the `RetrievalAPI` endpoint the orchestrator calls: `retrieve(error_context, k=10)` → list of ranked `CodeChunk` | - [ ] Build `MetadataExtractor`: function signatures, docstrings, type annotations — stored alongside each node for richer prompt context | - [ ] Build `ContextAssembler`: takes a list of `CodeChunk` objects, orders them logically, formats them into a clean prompt block |
| 07 | - [ ] Write a `RetrievalEval` harness: 20 seeded bugs, measure recall (did the root-cause file appear in top-10 results?) | - [ ] Implement `SyntaxErrorLocator`: when the agent writes invalid syntax, the AST parser pinpoints the exact broken bracket and reports it back | - [ ] Build `TokenBudgetConstraint`: trim the retrieved context to fit within the model's context window without cutting mid-function |
| 08 | - [ ] Write stress test: index a 10k-file open-source repo (e.g., `django`), measure indexing time and query latency | - [ ] Build `CrossLanguageEdge`: record edges where a Python file calls a TypeScript endpoint (REST boundary detection) | - [ ] Build `RelevanceFeedbackLoop`: after a successful run, upweight the nodes that appeared in the final fix for future queries on similar errors |
| 09 | - [ ] Add `IndexHealth` CLI command: `hydra index-status --repo PATH` — shows stale files, coverage %, last-indexed timestamp | - [ ] Write `ASTVisualizerCLI`: `hydra ast-graph --repo PATH --file auth.py` — pretty-print the call graph as ASCII | - [ ] Write `RetrievalComparisonTest`: same 20 bugs, run flat-embedding retrieval vs. graph-walk retrieval, compare recall numbers |
| 10 | - [ ] **Phase Ⅲ demo:** one bug that flat search misses, graph walk finds — show the exact query path through the graph | - [ ] **Phase Ⅲ demo:** show `hydra ast-graph` output for the demo repo | - [ ] **Phase Ⅲ demo:** publish the recall comparison table — flat embeddings vs. graph walk, on 20 real bugs |

---

### Phase Ⅳ — Three-Node Mesh

*Goal: the Phase Ⅰ CLI command runs across all three physical laptops over WiFi. Supervisor on A, inference on B, sandbox on C.*

| # | M. Yadav — Supervisor Network Core | Y. Jangra — Inference Worker Service | B. Kataria — Sandbox Worker Service |
|---|-------------------------------------|---------------------------------------|--------------------------------------|
| 01 | - [ ] Design the gRPC proto definitions: `OrchestratorService`, `InferenceService`, `SandboxService` — commit these as the v1 contract | - [ ] Wrap the Ollama inference engine as a gRPC `InferenceService` server on Node B | - [ ] Wrap the Docker sandbox as a gRPC `SandboxService` server on Node C |
| 02 | - [ ] Build the `SupervisorRouter`: dispatch `CodeTask` to Node B, `TestTask` to Node C, based on task type | - [ ] Implement streaming gRPC: stream inference tokens back to the supervisor as they generate, don't wait for completion | - [ ] Build `SecureTransfer`: before sending a repo slice to Node C, tar only the relevant subdirectory — don't send the whole repo |
| 03 | - [ ] Build the `HeartbeatMonitor`: ping each worker every 5s, mark as `UNHEALTHY` after 3 missed beats | - [ ] Implement `BackpressureController` on Node B: reject new requests with `UNAVAILABLE` if the current context is nearly full | - [ ] Build the sandbox gRPC handler: receive a repo slice, materialize it in a temp directory, execute, return structured results |
| 04 | - [ ] Build worker failover: if Node B goes `UNHEALTHY` mid-task, emit `NODE_LOST` event, pause execution, wait for reconnect | - [ ] Add request queuing on Node B: buffer incoming tasks during model warmup so they don't timeout immediately | - [ ] Add basic auth to the gRPC server: mutual TLS + pre-shared service token — no device on the LAN that doesn't have the token can connect |
| 05 | - [ ] Build `NodeRegistry`: Node B and Node C announce themselves with `hydra join --host IP --role` on startup | - [ ] Implement graceful shutdown on Node B: finish the in-flight inference before stopping, don't cut mid-generation | - [ ] Implement sandbox isolation hardening: no outbound network from containers, cgroup limits on CPU and RAM |
| 06 | - [ ] Build `ResourceMatrix`: supervisor tracks Node B VRAM usage and Node C container count; routes based on capacity | - [ ] Measure and document the round-trip latency added by the network hop — this number is important for honest benchmarking | - [ ] Build `ContainerPooling`: keep N warm empty containers ready to accept a job without cold-start overhead |
| 07 | - [ ] Build `EventBroadcaster`: all significant events (task dispatched, Reaper fired, test passed) go to the WebSocket stream | - [ ] Add automatic model reload on Node B if Ollama crashes: detect the dead process, restart it, re-warm the model | - [ ] Build `SandboxTelemetry`: emit container CPU, RAM, and disk I/O back to the supervisor's ResourceMatrix |
| 08 | - [ ] Write `ClusterHealthCLI`: `hydra cluster status` — show all connected nodes, their load, and their last heartbeat | - [ ] Write integration test: kill Node B mid-inference, confirm the supervisor emits `NODE_LOST` and pauses cleanly | - [ ] Write integration test: kill Node C mid-test, confirm the sandbox job is retried on restart |
| 09 | - [ ] Write network load test: sustain a 2-hour run on a real repo, measure packet loss, latency jitter, and recovery from a brief WiFi dropout | - [ ] Write `InferenceMetricsExporter`: publish tokens/sec, queue depth, and context utilization over gRPC to the supervisor | - [ ] Document the exact WiFi setup (channel, band, AP distance) used in the test — this matters for reproducibility |
| 10 | - [ ] **Phase Ⅳ demo:** three laptops on a table, one `hydra fix` command, all three terminals active — screen record the full run | - [ ] **Phase Ⅳ demo:** show the cluster status CLI output during the run | - [ ] **Phase Ⅳ demo:** show the teardown — all three nodes clean up gracefully after the task completes |

---

### Phase Ⅴ — War Room Dashboard

*Goal: a live, screen-recordable UI showing agent state, genealogy tree, node meters, and manual override controls.*

| # | M. Yadav — WebSocket Server & Backend Events | Y. Jangra — Frontend Components | B. Kataria — State Management & Controls |
|---|----------------------------------------------|----------------------------------|------------------------------------------|
| 01 | - [ ] Build the WebSocket event server on Node A: one connection per client, one event stream per active run | - [ ] Scaffold the Next.js 14 app with App Router, shadcn/ui components, and Tailwind — minimal, dark, terminal-inspired | - [ ] Build the Zustand store: normalize incoming WebSocket events into `currentRun`, `nodes`, `genealogy`, `logs` slices |
| 02 | - [ ] Define the full event schema: `TASK_DISPATCHED`, `AGENT_THOUGHT`, `TOOL_CALLED`, `TOOL_RESULT`, `REAPER_FIRED`, `BRANCH_CREATED`, `TEST_PASSED`, `RUN_COMPLETE` | - [ ] Build `LiveLogStream`: virtualized list of incoming agent thought/action/observation events, scrolls automatically | - [ ] Build `WebSocketClient`: auto-reconnects on drop, emits a `DISCONNECTED` banner in the UI, resumes stream on reconnect |
| 03 | - [ ] Build `JWT auth` for the dashboard WebSocket endpoint: generate a token with `hydra dashboard-token`, required in the WS URL | - [ ] Build `GenealogyTree`: interactive tree view of all attempted branches, color-coded (green=pass, red=dead, amber=active) | - [ ] Build `RunControlPanel`: Pause button → sends `PAUSE` command to the supervisor; Abort button → sends `ABORT`; Resume → sends `RESUME` |
| 04 | - [ ] Build `RunHistoryStore`: every completed run written to SQLite; endpoint to list past runs and serve their event log | - [ ] Build `NodeMeterPanel`: VRAM bar for Node B, container count for Node C, heartbeat indicator for all three | - [ ] Build `HumanGateModal`: when the supervisor emits `AWAITING_APPROVAL`, the UI blocks and shows the proposed change for human sign-off |
| 05 | - [ ] Build `LogExportEndpoint`: `GET /api/runs/{id}/export` → returns the full JSONL event log as a downloadable file | - [ ] Build `DiffViewer`: side-by-side diff of the current file edit with syntax highlighting (use `react-diff-viewer-continued`) | - [ ] Build `OverridePromptBox`: text field that injects a human instruction into the current REFLECT step mid-run |
| 06 | - [ ] Build streaming compression: compress large log payloads before sending over WebSocket to avoid browser frame drops | - [ ] Build `ConvergenceGauge`: real-time 0–1 gauge for the current approach's convergence score — turns red as it approaches 0.75 | - [ ] Build `SessionReplayViewer`: for any past run in history, step through events one at a time like a video scrubber |
| 07 | - [ ] Add `RateThrottler` on the WebSocket server: if events arrive faster than 60/sec, batch and debounce before sending to client | - [ ] Build `WarRoomMode`: fullscreen split view with Genealogy + LiveLog + NodeMeters + ConvergenceGauge all visible simultaneously | - [ ] Build `AlertSystem`: toast notifications for `REAPER_FIRED`, `NODE_LOST`, `TEST_PASSED`, `RUN_COMPLETE` — distinct visual style per type |
| 08 | - [ ] Write a `FuzzerTest` for the WebSocket server: send 10,000 random events per second, assert the server doesn't drop events or crash | - [ ] Build `MobileResponsiveness`: the dashboard must be readable on a phone screen (for checking a run while away from the desk) | - [ ] Build `ThemeToggle`: light/dark mode without a page reload — because dark mode is the default but not everyone wants it |
| 09 | - [ ] Build `MultiClientBroadcast`: two browsers can be connected simultaneously and see the same live run without interfering | - [ ] Write Playwright end-to-end tests for the UI: connect to a mock WebSocket, feed it a scripted event stream, assert UI state | - [ ] Write `BundleAudit`: `next build` output must stay under 300kB JS per route — enforce with a CI check |
| 10 | - [ ] **Phase Ⅴ demo:** screen record War Room Mode on a live three-node run — show the genealogy tree growing in real-time as the Reaper fires | - [ ] **Phase Ⅴ demo:** show the ConvergenceGauge going red and the `REAPER_FIRED` toast appearing | - [ ] **Phase Ⅴ demo:** show the human override in action — type a correction mid-run and watch the agent change direction |

---

### Phase Ⅵ — Benchmark, Harden, Ship

*Goal: real SWE-bench Lite numbers (failures published too), a clean repo, and a launch the developer community can actually trust.*

| # | M. Yadav — Hardening & Production Polish | Y. Jangra — Benchmarks & Documentation | B. Kataria — Launch & Community Setup |
|---|-------------------------------------------|----------------------------------------|---------------------------------------|
| 01 | - [ ] Run a 48-hour soak test across all three nodes on a real medium-sized repo — log every crash and fix every one | - [ ] Select a representative 50-issue slice from SWE-bench Lite; document the selection methodology | - [ ] Rewrite the README intro section now that there's a real demo — replace every claim with a link to the evidence |
| 02 | - [ ] Pin all dependency versions in `pyproject.toml` and `package.json`; run `pip-audit` and `npm audit` — fix critical CVEs | - [ ] Run HydraNet against all 50 issues; record pass/fail/reaper-fired/timeout for each | - [ ] Record the final demo video: unscripted, 2–3 minutes, War Room Mode on a real bug — raw > produced |
| 03 | - [ ] Write a `SetupVerifier` CLI command: `hydra doctor` — checks Docker, Ollama, Postgres, Node B/C connectivity, and reports clearly | - [ ] Publish the raw benchmark data in `/benchmarks/results.json` — every issue ID, outcome, number of attempts, tokens used | - [ ] Write `CONTRIBUTING.md`: how to set up a dev environment, how to run tests, PR standards, commit message convention |
| 04 | - [ ] Write the security hardening doc: sandbox network isolation, API token storage, what data leaves the machine (nothing, unless you configure a cloud fallback) | - [ ] Write a fair comparison table: HydraNet vs. Aider vs. OpenHands vs. Cline on the same 50 issues — use their published numbers where available | - [ ] Set up issue templates: Bug Report, Feature Request, Question — with enough structure that a first-time contributor can use them |
| 05 | - [ ] Build the single-command installer: `curl -sSf https://raw.githubusercontent.com/mohityadav8/hydranet/main/install.sh | bash` — tested on Ubuntu 22.04, Debian 12, WSL2 | - [ ] Write the architecture deep-dive doc in `/docs/architecture.md`: every decision and the reasoning behind it, including the wrong paths taken | - [ ] Write `SECURITY.md`: how to report a vulnerability, response timeline, what's in scope |
| 06 | - [ ] Write `hydra cluster init` for first-time setup: generates the pre-shared token, writes `hydra.config.json` on all three machines | - [ ] Write the model selection guide: what fits in 6GB, what fits in 8GB, what to do if you have less | - [ ] Set up GitHub Actions: `lint-and-test.yml` runs on every PR, `benchmark-nightly.yml` runs the 50-issue suite weekly |
| 07 | - [ ] Write `hydra update` command: pull the latest version, run migrations if the DB schema changed | - [ ] Write the `Getting Started in 30 minutes` tutorial: assumes three laptops and a WiFi router, nothing else | - [ ] Build the static documentation site (`/docs`) using Docusaurus — hosted on GitHub Pages |
| 08 | - [ ] Do a dependency audit: remove anything that isn't used; freeze the rest | - [ ] Write the prompt engineering guide: how to get better results from HydraNet by writing better issue descriptions | - [ ] Draft the Hacker News launch post: lead with the benchmark numbers and the Genealogy View, not the marketing pitch |
| 09 | - [ ] Final repo cleanup: remove all API keys, temp files, debug branches, TODOs that aren't filed as issues | - [ ] Write the failure analysis doc: which bugs HydraNet consistently fails on, and what the known limitations are | - [ ] Draft the r/LocalLLaMA post: hardware requirements, model choices, honest expectations — the community will validate or destroy the claims, so be accurate |
| 10 | - [ ] **Ship:** tag `v0.1.0`, push, publish to GitHub | - [ ] **Ship:** post the benchmark report alongside the release | - [ ] **Ship:** Hacker News, r/LocalLLaMA, r/MachineLearning — 7:00 AM PST on a Tuesday |

---

## Getting Started

*This is the target interface. Build this across Phase Ⅰ and Phase Ⅳ.*

```bash
# Install (Phase Ⅵ target)
curl -sSf https://raw.githubusercontent.com/mohityadav8/hydranet/main/install.sh | bash

# Or from source
git clone https://github.com/mohityadav8/hydranet
cd hydranet
uv sync

# First-time cluster setup — run once on Node A (M. Yadav's machine)
hydra cluster init
# → Generates hydra.config.json with a shared token
# → Prints the join command for Node B and Node C

# Node B and Node C — run this on each worker machine
hydra join --host 192.168.1.XX:50051 --role inference   # Node B
hydra join --host 192.168.1.XX:50051 --role sandbox     # Node C

# Check the cluster is healthy
hydra cluster status
# NODE A  supervisor  HEALTHY  6.2GB VRAM free
# NODE B  inference   HEALTHY  3.8GB VRAM free  Qwen2.5-Coder-7B loaded
# NODE C  sandbox     HEALTHY  0 containers running

# Index a repository
hydra index --repo ./my-project

# Fix a bug
hydra fix --repo ./my-project --issue "TypeError: Cannot read property 'id' of undefined in auth.ts:47"

# Open the War Room dashboard
hydra dashboard
# → http://localhost:3000  (token in hydra.config.json)
```

---

## Benchmarks

*Phase Ⅵ target. No results yet. This section will be updated with real numbers when the benchmark run completes.*

| Metric | Target | Actual (post Phase Ⅵ) |
|--------|--------|----------------------|
| SWE-bench Lite issues (50-issue slice) | Run all 50 | TBD |
| Issues resolved (pass@1) | Publish honestly | TBD |
| Mean attempts before resolution | — | TBD |
| Mean attempts before Reaper fires | < 4 | TBD |
| Reaper false-positive rate | < 5% | TBD |
| Mean tokens per resolved issue | — | TBD |
| Indexing speed (django repo, 1k files) | < 3 minutes | TBD |
| Retrieval latency p99 | < 200ms | TBD |

The failure cases will be documented in `/benchmarks/failures.md`. Issues that HydraNet consistently fails on are as important to publish as the ones it solves.

---

## Project Structure

```
hydranet/
├── packages/
│   ├── orchestrator/          # Node A — state machine, Reaper, routing, WebSocket server
│   │   ├── state_machine.py
│   │   ├── reaper.py
│   │   ├── convergence.py
│   │   ├── router.py
│   │   └── cli.py
│   ├── inference/             # Node B — model server, AST parser, retrieval
│   │   ├── server.py          # gRPC InferenceService
│   │   ├── ast_parser.py
│   │   ├── retrieval.py
│   │   └── prompts/           # versioned prompt templates
│   ├── sandbox/               # Node C — Docker manager, test executor
│   │   ├── server.py          # gRPC SandboxService
│   │   ├── docker_manager.py
│   │   ├── executor.py
│   │   └── checkpoint.py
│   ├── dashboard/             # Next.js frontend
│   │   ├── app/
│   │   ├── components/
│   │   │   ├── GenealogyTree.tsx
│   │   │   ├── ConvergenceGauge.tsx
│   │   │   ├── NodeMeters.tsx
│   │   │   └── WarRoomLayout.tsx
│   │   └── lib/
│   └── shared/                # Proto definitions, types, constants
│       ├── proto/
│       │   ├── orchestrator.proto
│       │   ├── inference.proto
│       │   └── sandbox.proto
│       └── types.py
├── benchmarks/
│   ├── results.json           # Phase Ⅵ — real numbers go here
│   └── failures.md            # documented failure cases
├── docs/
│   ├── architecture.md
│   ├── getting-started.md
│   └── model-selection.md
├── hydra.config.json.example
├── install.sh
├── docker-compose.yml         # Node A local services (Postgres, pgvector)
├── pyproject.toml
└── README.md
```

---

## Why we chose these tools and not the alternatives

| Decision | What we chose | What we considered | Why |
|----------|--------------|-------------------|-----|
| Orchestration | LangGraph | AutoGen, custom FSM | Cyclic graphs with explicit state are the right abstraction for retry/rollback. AutoGen is better for fully autonomous multi-agent but harder to add deterministic interception points. |
| Transport | gRPC | raw WebSocket, ZeroMQ, NATS | gRPC gives typed contracts (via .proto), streaming, and TLS without additional infrastructure. NATS would be better at higher node counts — not needed for three nodes. |
| Vector store | pgvector in Postgres | Qdrant, Weaviate | One service to run, one backup strategy, relational schema handles the edge table cleanly. Qdrant is worth switching to if query latency becomes a bottleneck after real benchmarking. |
| Code parser | Tree-sitter | regex, `ast` module, ANTLR | Tree-sitter handles syntax errors gracefully (unlike Python's `ast` module), supports 100+ languages, and has good Python bindings. |
| Auth | TLS + pre-shared token | Custom RSA handshake | Rolling your own crypto is an entire project. TLS is already audited, one dependency, done. |
| Container | Docker | systemd-nspawn, Firecracker | Docker has the widest support across the three operating environments (Windows WSL2, Ubuntu, etc.) that are likely on these machines. |

---

## What this project is not trying to be

- A replacement for Cline, Aider, or OpenHands. Those are excellent. Use them.
- A free, self-hosted Devin. Devin's strength is cloud compute, parallel sessions, and enterprise integrations. That's a different product category.
- A VRAM-pooling system that merges three GPUs into one. That's tensor-parallel inference, a separate engineering challenge.
- A tool that hits 70%+ on SWE-bench. The frontier models with 1M-token context windows running on datacenter GPUs are the right tool for that benchmark at scale. Seven-billion-parameter 4-bit models on laptops are not.

HydraNet does **one thing** the field doesn't yet do well: **it quits dead approaches on purpose, documents why, and forces genuinely new ones.** The genealogy view, the convergence score, and the Reaper Protocol are the entire point.

---

## License

MIT

---

## Contributing

Fork → branch → PR. See `CONTRIBUTING.md` (written in Phase Ⅵ, once the architecture is stable enough that contribution guidelines won't change weekly).

---

<div align="center">

Built by  **B. Kataria** · **M. Yadav** · **Y. Jangra**

*Three laptops. Zero cloud. One problem solved properly.*

</div>
