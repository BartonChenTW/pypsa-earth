# What this fork changes in PyPSA-Earth

This fork (`BartonChenTW/pypsa-earth`, branch `taiwan`) is PyPSA-Earth plus the changes below.
Everything else is unchanged upstream code.

- **Base:** upstream commit `91c7ca83`. Upstream has moved about 73 commits ahead since (as of 2026-09-26).
- **Study:** the Taiwan study that uses this fork (configs, Taiwan data, website) is
  [BartonChenTW/taiwan-energy-model](https://github.com/BartonChenTW/taiwan-energy-model).
- **Full diff:** `git diff 91c7ca83 taiwan -- scripts Snakefile data`.
- **In the code:** most edits carry a `Taiwan fork` comment.

Keep this list up to date when a patch is added, changed, or accepted upstream.

## 1. Taiwan-specific model options

These are options the Taiwan study needs. They are off unless a config turns them on.

| File | Change | Config keys |
| --- | --- | --- |
| `Snakefile` | The custom power plant file is configurable (the Taiwan fleet lives in the study repository) | `electricity.custom_powerplants_file` |
| `scripts/add_electricity.py` | New nuclear at candidate sites when no nuclear plant is left in the fleet; floating offshore wind turbine cost and lifetime | `electricity.nuclear_candidate_sites` |
| `scripts/build_renewable_profiles.py` | Minimum water depth, for floating offshore wind | `renewable.<carrier>.min_depth` |
| `scripts/prepare_sector_network.py` | Gas combined cycle with carbon capture; hydrogen- and ammonia-fired combined cycles; imports of hydrogen, ammonia, synthetic oil and synthetic methane at fixed prices | `sector.taiwan_power`, `sector.hydrogen_import` |
| `scripts/solve_network.py` | Caps and floors on total capacity per technology group and planning horizon; bounds on shares of annual generation; nuclear caps | `solving.options.capacity_total_MW`, `capacity_max_total_MW`, `nuclear_max_total_MW`, `generation_share` |
| `scripts/prepare_energy_totals.py`, `scripts/build_industry_demand.py` | Override demand growth rates per country and year | `demand_growth_override` |
| `scripts/prepare_transport_data_input.py` | Override transport data per country (e.g. number of cars) | `transport_data_override` |

## 2. Fixes to PyPSA-Earth bugs

These would help any PyPSA-Earth user, so they are candidates for upstream pull requests. None was
in upstream as of 2026-09-26. Upstream reworked some of these files, so check each against current
upstream before opening a pull request.

| File | Problem fixed | Upstream status |
| --- | --- | --- |
| `scripts/build_base_energy_totals.py`, `scripts/build_base_industry_totals.py` | The UN energy statistics report Taiwan as "Other Asia", which has no ISO code and was dropped, so all Taiwan demand was zero | not in upstream |
| `scripts/prepare_urban_percent.py` | The UNCTAD bulk file id changed (355 no longer exists); the id is now looked up | not in upstream |
| `scripts/prepare_ports.py` | The World Port Index download fails on certificate errors; a local copy is used | not in upstream |
| `scripts/add_brownfield.py` | Myopic runs: existing plants of the base year never retired | upstream reworked this file; check |
| `scripts/add_existing_baseyear.py` | Myopic runs: existing and new links with the same name made PyPSA drop all links | upstream reworked this file; check |
| `scripts/solve_network.py` | The land-use limit subtracted existing capacity twice | upstream reworked this file; check |
| `scripts/prepare_sector_network.py` | Converted biomass plants got an unlimited free biomass supply | upstream reworked this file; check |
| `scripts/add_electricity.py` | The IRENA statistics download URL was broken; a direct URL is used and a replaced cache file is kept | check |
| `scripts/cluster_network.py` | Retry when a shared Gurobi licence is at its use limit | probably too site-specific for upstream |

## 3. Data

| File | Change | Made by |
| --- | --- | --- |
| `data/demand/growth_factors_cagr.csv`, `efficiency_gains_cagr.csv`, `industry_growth_cagr.csv` | One Taiwan row appended to each | `data/build_sector_growth_tw.py` in taiwan-energy-model |

`data/custom_powerplants.csv` is the upstream file. The Taiwan fleet is in taiwan-energy-model
(`data/fleet/`), and the Taiwan configs point to it with `electricity.custom_powerplants_file`.

## 4. Housekeeping

- **`docs/`** holds only redirect pages from the old website address to https://bartonchentw.github.io/taiwan-energy-model/.
- **`AGENTS.md`**, the note at the top of `README.md`, and this file.
- **`.gitignore`** ignores `.venv` (the local conda environment).

## Not in the repository

These are local to the workstation and not tracked: the data bundle, the weather cutouts (`cutouts/`)
and all outputs (`resources/`, `networks/`, `results/`, `logs/`, `benchmarks/`).

## History

- **Before the split:** the combined repository (model plus Taiwan study) is tagged `pre-split-2026-09` on branch `pypsa-taiwan-dev`.
- **Where the study moved:** see `notes/log.md` and `notes/REPO_SPLIT_PLAN.md` in taiwan-energy-model.
