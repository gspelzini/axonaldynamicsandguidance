# Public analysis examples

This repository contains English-language examples for running the analyses reported in the paper. Example datasets are synthetic and must not be interpreted as experimental observations or scientific results.

## Kolmogorov-Smirnov analysis of deviated angles

Repository structure:

```text
axonaldynamicsandguidance/
├── README.md
├── requirements.txt
├── ks_deviated_angles_analysis.ipynb
├── data/
│   └── example_synthetic_deviated_angles.xlsx
```

The Excel workbook contains synthetic deviated angles in degrees, with one condition per column. The notebook automatically locates it when launched from the repository root, from the `notebooks` directory, or when the Excel file is placed directly beside the notebook. When loaded, it prints the complete resolved path and the condition names found in the workbook.

## Installation and execution

Python 3.10 or later is recommended.

```bash
git clone https://github.com/gspelzini/axonaldynamicsandguidance.git
cd axonaldynamicsandguidance
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
jupyter lab ks_deviated_angles_analysis.ipynb
```

On Windows, activate the environment with `.venv\Scripts\activate` instead.

The example is deliberately constructed to show both possible outcomes at `alpha = 0.05`: CONTROL differs significantly from TREATMENT_W, TREATMENT_X, and TREATMENT_Y, whereas CONTROL and TREATMENT_Z do not differ significantly. These outcomes are properties of the synthetic teaching example only.

### Select the conditions to compare

Open the notebook and edit `COMPARISONS` in the configuration cell. Each entry contains two Excel column names:

```python
COMPARISONS = [
    ("CONTROL", "TREATMENT_W"),
    ("CONTROL", "TREATMENT_Z"),
]
```

To compare other conditions, replace or add pairs. For example:

```python
COMPARISONS = [
    ("TREATMENT_W", "TREATMENT_X"),
    ("TREATMENT_Y", "TREATMENT_Z"),
]
```

Names must match the Excel headers exactly. The notebook checks for missing names, removes blank cells independently from each column, reports both sample sizes, and then runs `scipy.stats.ks_2samp` with a two-sided alternative.

### Use another workbook

The simplest option is to place the new workbook in `data/` and change `DATA_FILENAME` in the notebook:

```python
DATA_FILENAME = "my_deviated_angles.xlsx"
```

If the worksheet has another name, also change `SHEET_NAME`. Keep one condition per column and store one deviated-angle observation per cell. Extra blank cells are allowed when sample sizes differ.

If many pairwise hypotheses are tested, define the comparisons in advance and consider an appropriate multiple-testing correction. The notebook reports the unadjusted two-sample KS p-values, matching the simple pairwise procedure used in the original analysis.

## Wind-rose plots

`wind_rose_synthetic_angles.ipynb` reads the same synthetic deviated-angle workbook and generates one polar histogram per selected condition. It preserves the original 10-degree bins from -90 to 180 degrees, places 0 degrees at the top, and increases angles clockwise.

Select the conditions in the configuration cell:

```python
CONDITIONS = ["CONTROL", "TREATMENT_W", "TREATMENT_Z"]
```

Running the notebook saves a 300-dpi PNG and TIFF for every selected condition in `outputs/wind_rose/`.

## Initial-angle and velocity plots

`initial_angle_velocity_analysis.ipynb` reads
`data/example_synthetic_initial_angle_velocity.xlsx` and generates one
semicircular polar scatter plot per selected condition. Initial angle is shown
from 0 to 180 degrees, while radial position and point colour both represent
velocity in µm/min. All plots use the same 0–0.9 µm/min scale so conditions can
be compared visually.

The example workbook is in long format, with one paired observation per row:

- `Observation ID`
- `Condition`
- `Initial angle (degrees)`
- `Velocity (µm/min)`

The included conditions are `CONTROL`, `TREATMENT_W`, `TREATMENT_X`,
`TREATMENT_Y`, and `TREATMENT_Z`. They are entirely synthetic and their
differences are illustrative, not results from the paper.

Select which conditions to plot in the configuration cell:

```python
CONDITIONS = [
    "CONTROL",
    "TREATMENT_W",
    "TREATMENT_X",
    "TREATMENT_Y",
    "TREATMENT_Z",
]
```

To use another workbook, place it in `data/`, change `DATA_FILENAME`, and keep
the four column names listed above. Running the notebook saves one 300-dpi PNG
per selected condition in `outputs/initial_angle_velocity/`.
