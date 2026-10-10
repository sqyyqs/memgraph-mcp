# <Project name>

[Русская версия](README.ru.md)

An open MCP server for shared graph memory across multiple LLM agents is being developed on top of Neo4j. 
The system supports storing facts with provenance information (agent, source, event time, and recording time), 
temporal queries such as “state at time t,” retrieval of relevant subgraphs under a token-budget constraint, 
and conflict detection when new facts are added.

**Research question:** Can a shared graph memory system with provenance, temporal reasoning,
and token-budget-aware context retrieval preserve answer quality while reducing context size
and maintaining acceptable latency compared with existing solutions?

**Data:** The evaluation will use the LoCoMo conversational memory benchmark. 
The proposed system will be compared with Graphiti-MCP, Mem0, and a full-context baseline using the same open-source LLM.

**Required outcome:** A tested MCP server with a documented API and a reproducible experimental setup. 
The evaluation metrics will include answer quality measured by an LLM judge, tokens per query, p95 latency, 
and precision of conflict detection. The success criterion is answer quality within 2 percentage points of Graphiti-MCP,
no more than 70% of its token usage per query, and conflict-detection precision of at least 0.8.

**Desired outcome:** Extend the comparison to Mem0^g and dense RAG, run experiments with three random seeds 
and confidence intervals, and conduct ablation studies on temporal reasoning, subgraph retrieval versus full context, 
token-budget size, and model choice.

**Project artifacts:** An open repository with reproducible experiment runs, an MCP interface contract
for platform agents, and a technical report.

## Quick start

```bash
cp .env.example .env          # fill in the keys; .env is ignored
uv sync                       # installs the package and the dev tools from uv.lock
uvx pre-commit install        # gitleaks, large files, make check before every commit
make check                    # ruff, mypy, pytest
make run                      # one experiment: configs/smoke.yaml -> an MLflow run
make mlflow                   # MLflow UI over the local mlflow.db
```

Rename the package once: `make rename NAME=<your_package>`, then `uv lock`.

## Layout

```
src/<package>/       code that is imported and tested: config.py, pipeline.py, __main__.py
tests/               smoke test: the pipeline on a tiny input
configs/             experiment configs (YAML)
notebooks/           exploration; outputs may stay as a report of a result
docs/adr/            decision log, one file per decision
docs/meetings/       meeting records
docs/reading-log.md  papers read
data/                ignored; how to fetch the data is described below
results/             tables and figures exported from code
.github/             PR template, issue form for experiments, CODEOWNERS, CI
```

## Data

Where the data lives and how to fetch it: <fill in>.

## Experiments

Every run is recorded in MLflow (`MLFLOW_TRACKING_URI` in `.env`, the local `mlflow.db` by default). A results row is commit, config path, seed, run id, metrics.

| commit | config | seed | run id | mse |
|---|---|---|---|---|

## Checks

`make check` runs from three places: pre-commit on your machine, CI on every PR, the agent hook. Branch `<type>/<short-description>`, PR title `<type>: ...`; the types are feat, fix, refactor, docs, test, chore, exp.

## LLM assistants

Which agents you use and for what: <fill in>. Their instructions: [AGENTS.md](AGENTS.md); the shared lab rules are connected by `make overlay` ([nir-agent-overlay](https://github.com/Industrial-AI-Research-Lab/nir-agent-overlay)). You are responsible for code written with an agent.

## Guides

[Practice guides](https://github.com/Industrial-AI-Research-Lab/project-implementation-manual/blob/master/nir-requirements/recommendations/README.md) of the lab manual: papers, repository and code, task tracking, agent artifacts.

## Licence

The template is MIT. Choose the licence of your own code and replace LICENSE if needed.
