# Resume drafts (November 2026)

These are standalone LaTeX drafts based on `main.tex`. The original `main.tex` is unchanged.

## Variants
- `base-with-authorization.tex`: **identical resume content to main.tex**, with only a conditional work-authorization line added.
- `systems-platform.tex`: Go distributed systems, production reliability, Kafka, Kubernetes deployments, and control-plane projects.
- `ai-inference.tex`: LLM serving, continuous batching, scheduling, KV cache, and measured latency/throughput results.
- `agent-engineer.tex`: backend infrastructure plus the implemented SQLite task/message primitives in the in-progress Python agent. Does **not** claim RAG, LangChain, tool calling, or a completed agent loop.

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
