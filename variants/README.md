# Resume drafts (November 2026)

These are standalone LaTeX drafts based on `main.tex`. The original `main.tex` is unchanged.

## Variants
- `base-with-authorization.tex`: **identical resume content to main.tex**, with only a conditional work-authorization line added.
- `systems-platform.tex`: Go distributed systems, production reliability, Kafka, Kubernetes deployments, and control-plane projects.
- `ai-inference.tex`: LLM serving, continuous batching, scheduling, KV cache, and measured latency/throughput results.
- `agent-engineer.tex`: backend infrastructure plus the implemented SQLite task/message primitives in the in-progress Python agent. Does **not** claim RAG, LangChain, tool calling, or a completed agent loop.

## Role-specific skills and frameworks: 2026 market research

**Research date: 5 November 2026.** This is a qualitative synthesis of **current employer job descriptions** from frontier labs and AI/compute startups—not a statistical frequency count or a claim that every listing requires all named tools. Some example jobs are senior/staff; use them to understand the technology and problems, not to infer your level. Full role links are below.

**Priority legend:** **P0** = essential concept/ability for the role family; **P1** = frequently useful practical tools; **P2** = specialized or team-dependent. These priorities are **our interpretation**, not employer-defined requirements. Do **not** add a framework to a resume until you can show real use.

| Target | Core hiring signal | Common technologies seen in roles | Strong project evidence |
| --- | --- | --- | --- |
| **Distributed Systems Engineer** (`systems-platform.tex`) | Concurrency, RPC, consistency, fault tolerance, resource scheduling, diagnosis under load | Go/Python, gRPC, Kafka, PostgreSQL, Linux, Prometheus, OpenTelemetry, sometimes Ray | Deterministic failure recovery, idempotency, measured tail latency |
| **Platform / Compute Engineer** (`systems-platform.tex`) | Kubernetes controller design, Linux/networking, IaC, multi-tenancy, on-call reliability | Go, Kubernetes/CRDs, `controller-runtime`, Terraform, Helm, Argo CD, Cilium, Firecracker (sandbox teams) | Real operator or sandbox lifecycle, reconciliation and failure tests |
| **Inference Platform Engineer** (`ai-inference.tex`) | Reliable model APIs, routing, scheduling, capacity, autoscaling, SLOs | Python/Go, vLLM, SGLang, Ray Serve, KServe, TensorRT-LLM, NVIDIA Dynamo, Kubernetes | Multi-model gateway and reproducible serving measurements |
| **Inference Runtime/Performance Engineer** (`ai-inference.tex`) | Batching, KV cache, prefill/decode, accelerator profiling, memory/compute tradeoffs | PyTorch, llama.cpp, vLLM/SGLang, CUDA, Triton, Nsight, NCCL | Instrumented, measured runtime optimization with an honest baseline |
| **Applied Agent Engineer** (`agent-engineer.tex`) | Python, tool calling, model APIs, retrieval/context, safe integrations, evals | LangGraph, LangChain, MCP, LangSmith, FastAPI, pgvector/FAISS | End-to-end tool-using workflow with task-success regression tests |
| **Agent Systems/Harness Engineer** (`agent-engineer.tex`) | Durable orchestration, sandboxed execution, checkpoints, permissions, tracing | Handwritten loops or LangGraph, SQLite/Postgres, isolated execution, OpenTelemetry | Agent survives restarts; audited tool use and measured solve rate |

### A. Distributed Systems Engineer

- **P0 — language and fundamentals:** Deep Go concurrency (goroutines, channels, locks, `context`, cancellation, races, `pprof`); Python where relevant; TCP/networking, HTTP/gRPC, Protobuf, data structures, concurrency, backpressure, resource budgets and performance analysis.
- **P0 — distributed correctness:** Ordering, idempotency, retries, consistency, partial failures, coordination, durable state transitions, graceful recovery, partitioning, load balancing, scheduling, and explicit delivery guarantees. Explain a real failure mode and how you tested it.
- **P0 — production maturity:** PostgreSQL transactions/indexes, logs/metrics/traces, p95/p99, SLOs, load tests, incidents, postmortems, and capacity planning.
- **P1 — hands-on tools:** Kafka (consumer groups, offsets/replay, ordering), Redis, `grpc-go`, Kubernetes, OpenTelemetry, Prometheus/Grafana; Ray where distributed compute is the product.
- **P2 — selected teams only:** Raft/consensus internals, etcd/ZooKeeper/Consul, distributed storage, RDMA, eBPF/kernel work. These are not universal requirements of all distributed-systems jobs.
- **Your evidence today:** Dora's measured Go matching-loop latency improvement; Cashflo's idempotent ERP ingestion and deduplication; Elocity's WebSocket/Kafka infrastructure.
- **Next project proof:** Use `agent-sandbox` to simulate multiple hosts, inject crashes/partitions, implement leases/fencing and idempotent operations, then publish recovery/invariant tests. **The runtime is currently planned, not implemented.**

**Example jobs:** [Anthropic — Infrastructure (Distributed Systems)](https://job-boards.greenhouse.io/anthropic/jobs/4970314008-0), [Thinking Machines Lab — Infrastructure](https://jobs.ashbyhq.com/ThinkingMachines/998becdd-611f-44e5-a36d-bbfc13328ed9), [Anyscale — Ray Data](https://jobs.ashbyhq.com/anyscale/5104d34f-d58a-4165-bb30-9a96c8ff311c), [Modal — Systems](https://jobs.ashbyhq.com/modal/9b33ebe7-e829-4f03-97ba-5c94dbd7daf6).

### B. Platform Engineer / AI Compute Infrastructure

- **P0 — Kubernetes internals:** API server, etcd, controllers/reconciliation, CRDs/operators, scheduler, kubelet, RBAC, resource limits, deployment/rollback, cluster networking and control-plane failure behavior. A **controller author** is different from a Kubernetes deployment **user**.
- **P0 — Linux/cloud/networking:** Processes, namespaces, cgroups v2, containers/OCI, systemd; DNS, TLS, TCP, L4/L7 routing, ingress/egress, network isolation, VPC/IAM and AWS/GCP operations.
- **P0 — reliability:** Observability, tracing, autoscaling, capacity, on-call debugging, security boundaries, SLOs, incident response and recovery.
- **P1 — frameworks/tools:** Go `client-go` and `controller-runtime` (or Kubebuilder/Operator SDK); Helm, Terraform, Argo CD/Flux, GitHub Actions/GitLab CI, Prometheus/Grafana/OpenTelemetry, managed Kubernetes (EKS/GKE).
- **P1 — AI compute specialization:** GPU/accelerator resource placement, gang scheduling, topology awareness, preemption, GPU device plugins, Slurm, Kueue, Volcano, NCCL/network fabrics.
- **P2 — sandbox/serverless specialization:** Firecracker, KVM/QEMU, snapshots, copy-on-write, userfaultfd, eBPF, Cilium, nftables, multi-tenant VM isolation and startup time. Relevant at E2B/Modal-type infrastructure companies, but **not** an across-the-board platform prerequisite.
- **Your evidence today:** Kubernetes, Docker, Helm, CI/deployment support; Go systems work. No implemented Kubernetes operator or Firecracker backend in your current public repos.
- **Next project proof:** Complete a sandbox on a Linux/KVM host, then build a `Sandbox` CRD/reconciler in Go (status, finalizers, retry and cleanup semantics). Demonstrate kill/restart recovery with tests and metrics.

**Example jobs:** [Anthropic — Kubernetes Platform (London)](https://job-boards.greenhouse.io/anthropic/jobs/5211305008-87), [Together AI — Platform Infrastructure](https://job-boards.greenhouse.io/togetherai/jobs/5180690007), [E2B — Platform](https://jobs.ashbyhq.com/e2b/f59dfa81-5b38-44da-90f5-0319eb31b627), [Perplexity — Inference & Training Platform](https://jobs.ashbyhq.com/perplexity/4b8347e9-615e-4176-bfb8-3be9025f9f27).

### C. AI Inference Engineer: platform vs. runtime

**Distinguish these hiring lanes:** *Inference platform* emphasizes reliable distributed serving, deployment, routing, autoscaling, quotas and SLOs. *Inference runtime/performance* emphasizes model execution, GPU efficiency, kernels and KV-memory tradeoffs. Several platform postings welcome strong distributed-systems engineers without requiring existing CUDA experience; performance postings may not.

- **P0 — model-serving fundamentals:** Python/Go/C++ as appropriate, PyTorch fundamentals, attention and transformer execution; gRPC/HTTP streaming, client cancellation, priority/admission control, queueing, retry/load shedding, model routing, authentication, metering, quotas and deployment reliability.
- **P0 — runtime fundamentals:** Prefill vs. decode, continuous batching, KV-cache lifecycle, prefix reuse, context lengths, caching, dynamic scheduling, throughput vs. latency and memory vs. compute bottlenecks.
- **P0 — measurement:** TTFT, ITL/TPOT, p50/p95/p99 end-to-end latency, tokens/sec, SLO goodput, GPU utilization and memory use, cost/tokens per GPU-hour. Pin hardware, model, precision, load distribution, software version, seed and run count for reproducible comparisons.
- **P1 — frameworks in job descriptions:** `vLLM`, `SGLang`, `TensorRT-LLM`, `Triton Inference Server` (serving), `Ray Serve`, `KServe`, `NVIDIA Dynamo`, `llm-d`, Kubernetes. **Learn one deeply** and know how it differs from another; no job requires mastering all of these.
- **P1 — advanced runtime features:** Paged/prefix KV caching, chunked prefill, cache-aware routing, quantization (FP8/FP4/AWQ/GPTQ), speculative decoding, LoRA, guided/structured output, prefill/decode disaggregation and horizontal/multi-model placement.
- **P2 — accelerator/performance lane:** GPU architecture/HBM, CUDA or **Triton GPU programming language** (not the same thing as *Triton Inference Server*), Nsight Systems/Compute, PyTorch Profiler, NCCL, NVLink/NVSwitch, InfiniBand/RDMA and tensor/pipeline/expert parallelism.
- **Your evidence today:** Go control plane, gRPC Python llama.cpp worker, continuous batching, per-sequence KV-cache management, request admission and in-repo benchmark comparisons. Benchmark manifest/trial limitations are openly documented; don't claim GPU-kernel wins or vLLM parity.
- **Next project proof:** Make benchmarks reproducible; instrument per-step prefill/decode/KV usage; implement one cache/scheduler optimization and compare against a baseline; then add vLLM/SGLang reference comparisons and GPU profiling. Your planned multi-model worker lifecycle is well aligned with *platform* roles.

**Example jobs:** [Anthropic — Inference Infrastructure](https://job-boards.greenhouse.io/anthropic/jobs/5245851008-110), [Thinking Machines Lab — Inference](https://jobs.ashbyhq.com/ThinkingMachines/4087e0f6-4295-419a-ba02-08e95a74ceea/), [OpenAI — GPT Infrastructure](https://openai.com/careers/software-engineer-gpt-infrastructure-san-francisco/), [Baseten — Inference Platform](https://jobs.ashbyhq.com/baseten/14c4a663-0b1f-4c11-93ff-1359741ee456/), [Baseten — Inference Performance](https://jobs.ashbyhq.com/baseten/7cb19a05-8e5b-44cf-b3e7-19949e2eff04/), [Prime Intellect — Inference](https://jobs.ashbyhq.com/PrimeIntellect/abfa70f7-a6f1-44d2-a6c1-560e1c8477d4/).

### D. Agent Engineer: applied agents vs. agent systems

**Distinguish these hiring lanes:** *Applied agent* roles prioritize useful model/tool workflows, retrieval/context, evaluations and user impact. *Agent harness/runtime* roles prioritize long-horizon orchestration, secure execution, durable state, tool infrastructure and experimentally measured solve rate.

- **P0 — software and model integration:** Strong Python, async APIs, OpenAI/Anthropic model SDKs or equivalents, structured JSON outputs, tool/function calling, streaming/SSE, prompts/context-window management, token/cost limits, retries, timeouts and authentication.
- **P0 — robust orchestration:** Plan-act-observe loops, deterministic tool adapters, state machines, checkpoints/resume, idempotent actions, job queues, approval/permission gates, prompt-injection defenses, safe sandbox execution and auditing.
- **P0 — evaluation:** Real-world task-success and tool-call accuracy, held-out test sets, deterministic and model-based graders, traces, error analysis, ablations, regression suites, latency and **cost per successful task**. A demo is not a substitute for evaluations.
- **P1 — frameworks/protocols:** `LangGraph` (stateful orchestration), `LangChain` and `LlamaIndex` (integrations/retrieval), `LangSmith` or OpenTelemetry tracing, **MCP** (tool integration **protocol**, not agent framework), FastAPI, Postgres/SQLite, Redis/queues.
- **P1 — retrieval/context (especially applied agents):** Embeddings, chunking, keyword+vector hybrid search, reranking, grounding, citations, retrieval recall/precision; implementations such as `pgvector`, FAISS, Pinecone or Weaviate. **Not every agent requires RAG**.
- **P2 — specialized:** Multi-agent topologies, browser/computer use, speech/voice agents, long-term memory, multi-tenant agent platforms, RL/post-training loops. Build only when a target role demands it.
- **Your evidence today:** The `agent` repository currently implements SQLite task/message primitives and tests; the agent loop, reminders and policy/recovery remain a **plan**. `agent-sandbox` is pre-alpha. Don't add LangGraph/MCP/RAG/working-agent claims until built.
- **Next project proof:** Build a **single coding/repository agent** that reads an issue, retrieves code, executes limited tools in a sandbox, runs tests and proposes a patch; measure task outcomes on fixed cases, log trajectories, evaluate failure/safety cases, and resume from a killed process. Implement in a handwritten loop first, optionally demonstrate a LangGraph version second.

**Example jobs:** [OpenAI — Codex Agent Systems](https://openai.com/careers/ai-systems-engineer-codex-agents-san-francisco/), [OpenAI — Applied Codex Agent](https://openai.com/careers/applied-ai-engineer-codex-core-agent-san-francisco/), [Anthropic — Applied AI Demo Engineer](https://job-boards.greenhouse.io/anthropic/jobs/5455961008), [Planera — AI Agent Engineer](https://jobs.ashbyhq.com/planera/d68c8a09-a11d-409e-85ca-5d434caf3fc8/), [BIO — AI Engineer](https://jobs.ashbyhq.com/bio/80145424-4809-46b2-99b9-8aae501be616/), [Moss — Applied AI Engineer](https://jobs.ashbyhq.com/moss/a4cd2807-aabc-4dfb-9256-0b3736582efe).

### Adjacent specialties: learn selectively, not as prerequisites

| Specialty | Commonly mentioned techniques/frameworks | One portfolio experiment |
| --- | --- | --- |
| **Training infrastructure** | PyTorch DDP/FSDP, `torch.distributed`, DeepSpeed/ZeRO, Megatron-LM, activation checkpointing, mixed precision, NCCL, Slurm/Ray, GPU tracing | Train a small transformer on 1 vs. multiple GPUs; chart step time, memory use, scaling and checkpoint/resume |
| **RL/post-training infrastructure** | Rollout generation, reward/verifier computation, PPO/GRPO, weight synchronization, `verl`, Ray, vLLM/SGLang, FSDP | Small test-verifiable environment with rollouts, reward and a measured improvement loop |
| **GPU kernel engineering** | CUDA, Triton language, memory hierarchy, shared memory, coalescing, CUTLASS, occupancy, Nsight Systems/Compute | One measured RMSNorm or softmax kernel against PyTorch across shapes with correctness checks |

Examples: [Black Forest Labs — Research Engineer](https://jobs.ashbyhq.com/black-forest-labs/362e79d1-1b62-4a28-ba4d-46484cbfcf3c), [Remanence — Post-training/RL](https://jobs.ashbyhq.com/remanence/04df31b4-049e-4798-b5c4-a7ef3a7fc157), [Baseten — Inference Performance](https://jobs.ashbyhq.com/baseten/7cb19a05-8e5b-44cf-b3e7-19949e2eff04/). These tracks are **optional specializations**, not requirements for all inference/platform jobs.

### Evidence checklist before changing resume skills

| Role | Demonstrated already | Next gap to close | Do not claim yet |
| --- | --- | --- | --- |
| Distributed | Go, Kafka, gRPC, WebSockets, idempotent ingestion, latency optimization | Distributed failure/recovery experiments | Production consensus systems if not used |
| Platform | Kubernetes/Helm/CI deployment support, Go services | Kubernetes operator, Linux sandbox lifecycle | Custom operator, Firecracker runtime |
| Inference | Serving scheduler, llama.cpp, continuous batching, KV management, tests | Reproducible profiling and comparative benchmark | CUDA kernel performance or multi-GPU scaling |
| Agent | SQLite task/message storage and tests | Working agent loop, real tool use, evals and retrieval | LangGraph/MCP/RAG or completed agent runtime |
| Advanced AI | Basic transformer and DDP code | Measured distributed-training, RL or GPU-kernel experiment | Large-scale training or production RL infrastructure |

**Practical rule:** select the 5–8 most relevant **supported** skills for a specific job description; use the missing-skills columns as your learning roadmap, not a keyword-stuffing list. Update the actual LaTeX skill entries only after shipping the corresponding project features. Dates, location, seniority and postings may change after this research snapshot.

## Compile UK / Canada versions

From the repository root, run:

```sh
mkdir -p build
# Default is UK (HPI)
pdflatex -interaction=nonstopmode -halt-on-error -output-directory=build -jobname=systems-platform-uk variants/systems-platform.tex
# Define CANADA to select the Canadian employer-sponsored wording
pdflatex -interaction=nonstopmode -halt-on-error -output-directory=build -jobname=systems-platform-ca '\def\CANADA{1}\input{variants/systems-platform.tex}'
```

Replace `systems-platform` with `ai-inference`, `agent-engineer`, or `base-with-authorization`. Each role draft supports both countries; no visa is claimed as already held.

**Work authorization language:** Canada GTS Category B is an employer-side process, and category/occupation matching depends on the role and employer. UK HPI is an unsponsored route but still requires a successful visa application (and Ecctis/other eligibility checks). The candidate does **not** currently claim work authorization in either jurisdiction.

## Claim checks before applying
- Inference benchmark figures are based on a controlled in-repository workload, not a comparison to vLLM or measured production GPU deployment; the benchmark metadata still needs strengthening.
- `agent-sandbox` is not on the resume: the Firecracker runtime is planned, not operational.
- The agent is labeled **in progress** and claims only implemented SQLite persistence and tests.
- Confirm document remains one page after local compilation; PDF outputs are not checked into this PR.
