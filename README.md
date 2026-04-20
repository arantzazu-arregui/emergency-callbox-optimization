![](data/emergency_callbox_optimization.png)

# Emergency Call Box Optimization

![GitHub release (latest by date including pre-releases)](https://img.shields.io/github/v/release/arantzazu-arregui/emergency-callbox-optimization?include_prereleases)
![GitHub last commit](https://img.shields.io/github/last-commit/arantzazu-arregui/emergency-callbox-optimization)
![GitHub pull requests](https://img.shields.io/github/issues-pr/arantzazu-arregui/emergency-callbox-optimization)
![GitHub](https://img.shields.io/github/license/arantzazu-arregui/emergency-callbox-optimization)
![contributors](https://img.shields.io/github/contributors/arantzazu-arregui/emergency-callbox-optimization)
![codesize](https://img.shields.io/github/languages/code-size/arantzazu-arregui/emergency-callbox-optimization)

> A data science and optimization project that digitizes McGill University's downtown campus map and uses mixed-integer programming to decide which emergency call boxes to keep, replace, or newly install.

## Project Overview

This project studies how to improve emergency phone coverage on McGill University's downtown campus while controlling installation cost. The workflow starts with a campus map, extracts spatial information such as existing call box locations, building outlines, and night-route geometry, and then feeds those inputs into a Gurobi optimization model.

In the current notebook snapshot, the model works with `123` possible call box locations, including `23` existing sites and `100` new candidate sites, and evaluates coverage across `98` demand points sampled along campus routes. The target is to satisfy a weighted coverage threshold of `alpha = 0.65`.

Team members:

- Rui Zhao
- Serena Sun
- Arantzazu Arregui Gonzalez
- Chloee Liew
- Nicholas Stanfield

## Installation and Setup

This repository is notebook-driven, so the main setup task is preparing a Python environment with the required scientific and optimization libraries.

### Codes and Resources Used

- **Editor Used:** Jupyter Notebook / JupyterLab
- **Python Version:** `3.11.14` in both checked-in notebooks
- **Optimization Solver:** Gurobi Optimizer `12.0.3` is referenced by the optimization notebook output
- **GitHub Repository:** [arantzazu-arregui/emergency-callbox-optimization](https://github.com/arantzazu-arregui/emergency-callbox-optimization)

### Python Packages Used

- **General Purpose:** `math`, `csv`
- **Data Manipulation:** `pandas`, `numpy`
- **Data Visualization:** `matplotlib`, `seaborn`
- **Image and PDF Processing:** `fitz` (PyMuPDF), `Pillow`, `opencv-python`
- **Machine Learning / Optimization:** `gurobipy`

### Suggested Setup

1. Clone the repository.
2. Create a Python 3.11 environment.
3. Install the libraries used in the notebooks.
4. Ensure Gurobi is installed and licensed before running the optimization notebook.
5. Open the notebooks in the `notebooks/` folder and run them from top to bottom.

Example installation commands:

```bash
conda create -n emergency-callbox python=3.11
conda activate emergency-callbox
pip install pandas numpy matplotlib seaborn pymupdf pillow opencv-python jupyter
pip install gurobipy
```

## Data

The project relies on a campus map plus derived spatial datasets stored as CSV files.

### Source Data

- **`data/campus_map.pdf`**: Source campus map used to identify buildings, routes, and call box locations.
- **`data/existing_callbox_current_state.csv`**: Current condition data for existing emergency phones, including phone type and state.
- **`data/exisiting_and_candidate_callbox_coords.csv`**: Combined table of existing and generated candidate call box locations.
- **`data/demand_point_coords.csv`**: Demand point coordinates with weights used in the optimization model.
- **`data/building_outline_vertices.csv`**: Traced building polygon boundaries.
- **`data/solid_line_vertices.csv`**: Main night-route vertices.
- **`data/dotted_line_vertices.csv`**: Feeder-route vertices.

### Data Acquisition

The acquisition process is implemented in `notebooks/pdf_map_coord_extraction.ipynb`. The notebook converts the campus PDF into an image, detects existing call box icons, traces route and building geometry, generates `100` candidate locations along building boundaries, and writes the combined location file to `data/exisiting_and_candidate_callbox_coords.csv`.

Demand points are then generated along the mapped routes, and the notebook reports `98` total demand points in the saved output.

### Data Preprocessing

Preprocessing in this project is mostly spatial rather than tabular. The workflow standardizes the map into a pixel-based coordinate system, builds route polylines, assigns weighted demand points, and prepares clean CSV inputs for the optimization model. The optimization notebook then merges those derived coordinates with the current-state file for the existing call boxes.

## Code Structure

The repository is compact and organized around two notebooks plus the generated data assets:

```bash
.
|-- data
|   |-- building_outline_vertices.csv
|   |-- campus_map.pdf
|   |-- demand_point_coords.csv
|   |-- dotted_line_vertices.csv
|   |-- emergency_callbox_optimization.png
|   |-- exisiting_and_candidate_callbox_coords.csv
|   |-- existing_callbox_current_state.csv
|   `-- solid_line_vertices.csv
|-- notebooks
|   |-- callbox_optimization_gurobi.ipynb
|   `-- pdf_map_coord_extraction.ipynb
`-- README.md
```

Key files:

- **`notebooks/pdf_map_coord_extraction.ipynb`**: Extracts and prepares the spatial inputs from the campus map.
- **`notebooks/callbox_optimization_gurobi.ipynb`**: Builds and solves the mixed-integer optimization model and exports the final visualization.
- **`data/emergency_callbox_optimization.png`**: Final solution plot generated by the optimization notebook.

## Results and Evaluation

The optimization notebook solves a mixed-integer model that minimizes installation cost while enforcing a weighted coverage requirement.

Current reported results from the checked-in notebook output:

- **Possible phone locations:** `123`
- **Existing locations:** `23`
- **Candidate new locations:** `100`
- **Demand points:** `98`
- **Demand points covered:** `60/98` (`61.2%`)
- **Weighted coverage:** `71.0/109.0` (`65.1%`)
- **Installation costs:** `$234,500.00`

The final weighted coverage slightly exceeds the model target of `65%`, which means the current solution meets the required service level while prioritizing the more heavily weighted demand areas. At the same time, the raw coverage result shows a clear tradeoff: reaching the target still leaves some locations uncovered and requires a substantial capital investment.

## Future Work

- Convert the pixel-based map coordinates into a GIS-ready spatial reference system.
- Test multiple budget levels and coverage thresholds instead of a single `alpha = 0.65` scenario.
- Replace straight-line distance with route-based walking distance or visibility-aware coverage.
- Add robustness checks using incident data, pedestrian traffic, or stakeholder safety priorities.
- Package the environment in a `requirements.txt` or `environment.yml` file for easier reproduction.

## Acknowledgments/References

- McGill campus map data used as the basis for the spatial extraction workflow.
- Field-observation data captured in `existing_callbox_current_state.csv`.
- Gurobi for solving the optimization model.
- README section structure adapted from the data science README template shared by [pragyy/datascience-readme-template](https://github.com/pragyy/datascience-readme-template).

## License

This repository currently does not include a `LICENSE` file in the checked-in source. Before wider reuse or contribution, add an explicit project license and document any reuse restrictions associated with the campus map and field-observation data.
