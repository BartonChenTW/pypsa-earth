# AGENTS.md

This folder is the PyPSA-Earth model checkout for the Taiwan study (fork BartonChenTW/pypsa-earth,
branch `taiwan`: upstream PyPSA-Earth plus the Taiwan-specific options, marked `Taiwan fork` in the
code). It holds the Python environment `.venv`, the data bundle, cutouts and all outputs.

The study itself (Taiwan configs and data, sandbox, exporter, website, notes) is the repository
BartonChenTW/taiwan-energy-model, checked out next to this folder at
`F:\Barton\Repositories	aiwan-energy-model`. Read its `AGENTS.md` for how to run everything;
Snakemake runs here with configs from there:

```bash
python -m snakemake -j 1 solve_all_networks --configfile ../taiwan-energy-model/config/config_tw_test2_highs.yaml -n
```

The state before the split is tagged `pre-split-2026-09` (branch `pypsa-taiwan-dev`).
