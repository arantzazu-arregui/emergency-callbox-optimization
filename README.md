# Emergency Call Box Optimization

This repository contains a notebook-based emergency call box optimization project for the McGill downtown campus. The workflow starts from a campus map, generates supply and demand inputs, and solves a Gurobi model to decide which emergency phones to keep, replace, or install.

![Emergency call box optimization result](data/emergency_callbox_optimization.png)

## Team

- Rui Zhao
- Serena Sun
- Arantzazu Arregui Gonzalez
- Chloee Liew
- Nicholas Stanfield

## Notebook Workflow

### `notebooks/pdf_map_coord_extraction.ipynb`

This notebook prepares the spatial inputs used by the optimization model.

- loads `data/campus_map.pdf`
- detects existing emergency call box icons from the map
- uses traced building outlines to generate `100` candidate call box locations
- samples demand points along traced routes every `50` meters
- writes the processed inputs to the `data/` folder

Generated data used by the model:

- `data/exisiting_and_candidate_callbox_coords.csv`
- `data/demand_point_coords.csv`
- `data/building_outline_vertices.csv`
- `data/solid_line_vertices.csv`
- `data/dotted_line_vertices.csv`

The notebook output reports `98` total demand points.

### `notebooks/callbox_optimization_gurobi.ipynb`

This notebook formulates and solves the emergency phone allocation model in Gurobi.

Model inputs from the checked-in notebook snapshot:

- `123` total possible locations
- `23` existing phone locations
- `100` candidate locations
- `98` demand points
- weighted minimum coverage target `alpha = 0.65`
- phone coverage radii `150, 100, 60, 45, 20`
- phone costs `$19,500`, `$10,500`, `$3,500`, `$2,000`, and `$0` for keeping the old phone

Reported notebook results:

- total phones in period 1: `38`
- demand points covered: `60 / 98` (`61.2%`)
- weighted coverage: `71.0 / 109.0` (`65.1%`)
- installation costs: `$234,500.00`

The notebook also saves the visualization to `data/emergency_callbox_optimization.png`.

## Repository Layout

- `notebooks/pdf_map_coord_extraction.ipynb`: spatial preprocessing notebook
- `notebooks/callbox_optimization_gurobi.ipynb`: optimization notebook
- `data/campus_map.pdf`: source map
- `data/building_outline_vertices.csv`: traced building vertices
- `data/solid_line_vertices.csv`: traced main route vertices
- `data/dotted_line_vertices.csv`: traced feeder route vertices
- `data/exisiting_and_candidate_callbox_coords.csv`: existing and candidate phone locations
- `data/existing_callbox_current_state.csv`: current phone condition and type data
- `data/demand_point_coords.csv`: generated demand points and weights
- `data/emergency_callbox_optimization.png`: saved optimization figure

## Requirements

The notebooks use:

- `jupyter`
- `pandas`
- `numpy`
- `matplotlib`
- `seaborn`
- `opencv-python`
- `Pillow`
- `PyMuPDF`
- `gurobipy`

`gurobipy` requires a working Gurobi installation and license.

## How To Run

1. Create a Python environment and install the required packages.
2. Launch Jupyter Notebook or JupyterLab from the repository root.
3. Run `notebooks/pdf_map_coord_extraction.ipynb` to regenerate the spatial CSV inputs.
4. Run `notebooks/callbox_optimization_gurobi.ipynb` to solve the model and regenerate the output figure.

Note: keep the filename `data/exisiting_and_candidate_callbox_coords.csv` as-is unless you also update the notebook references.
