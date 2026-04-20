# Emergency Call Box Optimization

![Emergency call box optimization result](data/emergency_callbox_optimization.png)

## Project Overview

This project studies how to allocate emergency phones across McGill University's downtown campus. The core problem is to improve emergency coverage while limiting total cost. Existing call boxes are uneven in quality, some are outdated or rusted, and the current network does not cover all parts of the campus night-route system effectively.

Using the campus map as the starting point, the project digitizes existing call box locations, building outlines, and route geometry, then uses those inputs in an optimization model that decides which phones to keep, replace, or newly install.

**Team**

- Rui Zhao
- Serena Sun
- Arantzazu Arregui Gonzalez
- Chloee Liew
- Nicholas Stanfield

## Formulation

Following the report, the optimization problem is modeled as a mixed-integer decision problem that minimizes total cost while meeting a minimum weighted coverage requirement.

In the checked-in notebook snapshot, the model uses:

- `123` total possible phone locations
- `23` existing locations
- `100` candidate locations
- `98` demand points
- a weighted coverage target of `alpha = 0.65`

The formulation allows the model to:

- keep an existing phone at its current location
- replace an existing phone with a better phone type
- install a new phone at a candidate location

The current notebook uses five phone types with coverage radii:

- `150`, `100`, `60`, `45`, `20`

and costs:

- `$19,500`
- `$10,500`
- `$3,500`
- `$2,000`
- `$0` for keeping the old phone type

The report provides the main modeling logic used in the notebooks:

- demand points represent locations along the campus route network
- straight-line distance is used to determine whether a demand point is covered
- higher-traffic areas receive higher weights
- old phones may be retained, but keeping lower-quality legacy phones is penalized
- the old phone type is not installed at new locations
- existing phones are not downgraded to worse phone types

## Repository Structure

```text
├── data/
│   ├── campus_map.pdf                               # Source map (existing call box locations, buildings, and routes)
│   ├── building_outline_vertices.csv                # Traced campus building boundaries
│   ├── solid_line_vertices.csv                      # Main night route vertices
│   ├── dotted_line_vertices.csv                     # Feeder route vertices
│   ├── exisiting_and_candidate_callbox_coords.csv   # All 123 phone locations
│   ├── demand_point_coords.csv                      # 98 emergency phone demand points with weights
│   └── existing_callbox_current_state.csv           # Phone types & conditions at existing locations (Field Observations)
│
├── notebooks/
│   ├── pdf_map_coord_extraction.ipynb               # Step 1: Data extraction
│   └── callbox_optimization_gurobi.ipynb            # Step 2: Optimization model
│
└── README.md
```

## Results

The preprocessing notebook generates the spatial inputs used by the model and reports:

- `100` candidate call box locations
- demand points sampled every `50` meters
- `98` total demand points

The optimization notebook reports the following solution for the current checked-in model run:

- total phones in period 1: `38`
- demand points covered: `60 / 98` (`61.2%`)
- weighted coverage: `71.0 / 109.0` (`65.1%`)
- installation costs: `$234,500.00`

This solution slightly exceeds the model's weighted coverage target of `65%`, which suggests the optimizer is meeting the required service level while concentrating coverage on the more heavily weighted demand points. In practical terms, the result shows that achieving the current target still requires a substantial investment and does not cover every demand location, indicating a tradeoff between cost and broad geographic coverage.

The final plot is saved to `data/emergency_callbox_optimization.png`.
