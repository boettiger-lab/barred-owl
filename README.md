# barred-owl

AI-powered interactive map app for the **Barred Owl Prioritization Tool** (Northern
Spotted Owl range), built on [geo-agent](https://github.com/boettiger-lab/geo-agent)
via the [geo-agent-template](https://github.com/boettiger-lab/geo-agent-template).

**Live:** https://barred-owl.nrp-nautilus.io

The map shows the tool's **input layers** (owl occupancy, carrying capacity, fire risk,
refugia) styled per variable; the LLM agent queries the **prioritizr priority scores** for
all 2,754 management scenarios (`scores.parquet`) via the DuckDB MCP server and can map
the results. Data: [`public-barred-owl`](https://s3-west.nrp-nautilus.io/public-barred-owl/stac-collection.json)
STAC collection (mirror of <https://barredowltool.russell.wisc.edu>).

## Configuration

- `layers-input.json` — datasets, map layers (thematic aliases of the `inputs-pmtiles`
  asset), view, welcome prompts.
- `system-prompt.md` — agent domain context (inputs vs. scores, scenario keys, the
  `NSO_ccap` hex-aggregation caveat).
- `index.html` — HTML shell (core JS/CSS from CDN).
- `k8s/` — Kubernetes deployment (`biodiversity` namespace).

See [AGENTS.md](AGENTS.md) and the [geo-agent docs](https://boettiger-lab.github.io/geo-agent/)
for the full configuration and deployment reference.

## Deploy

```bash
git add <files> && git commit -m "…" && git push          # main is the live branch
kubectl rollout restart -f k8s/deployment.yaml            # init container re-clones main
kubectl rollout status  -f k8s/deployment.yaml
```
