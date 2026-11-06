# Electrification of Road Freight Transport in the Port of Rotterdam: Assessment of Charging Feasibility

**Date**: November 2026
**Course:** TIL6022 - TIL Python Programming
**Group** Group 21
**Repository link**: https://github.com/sandrazhu21/-TIL6022-TIL-Python-Programming-Group-21

# 1. Overview

This project examines the feasibility of electrifying representative road freight operations serving the Port of Rotterdam (PoR) hinterland. It assess whether electric trucks operating over short, medium, and long-distance duties can meet their daily energy requirements through overnight depot charging alone, or whether additional opportunity charging at terminals during operational dwell times is needed.

Using Python-based analysis, the project compares the energy required by different truck duties with the energy available from the selected battery and charging configurations. Sensitivity analysis explores how the results change with key assumptions, including battery capacity, energy consumption, travel distance, and terminal dwell time. The resulting calculations and visualizations are intended to inform charging infrastructure planning for freight operations.

**Main Research Question**

For representative short-, medium-, and long-distance truck duties serving the Port of Rotterdam, which can be completed with overnight depot charging alone, and which require additional opportunity charging during scheduled dwell periods at origin and destination?  

**Sub questions**

    1. What are the daily driving energy requirements of the representative short-, medium-, and long-distance truck duties? 

    2. Which combinations of operating profile and charging configuration allow the truck to complete its daily duty without falling below the minimum state of charge? 

    3. For the configurations that allow completion of the daily duty, is the remaining time before the next departure sufficient to restore the battery to its initial state of charge using depot charging? 

    4. How sensitive is duty feasibility to changes in energy consumption, destination dwell time, and intermediate port-terminal dwell time?

# 2. Repository Content

The repository contains the input data, analysis notebook(s), and project figures.

```text
repository-root/
├── README.md
├── Final_code(1).ipynb
└── report_outputs/                   # generated when the notebook runs
    ├── feasibility_results.csv
    ├── duty_feasibility_matrix.png
    ├── repeatability_matrix.png
    ├── trajectories.png
    ├── min_soc_bar.png
    ├── consumption_sensitivity_soc.png
    ├── destination_dwell_sensitivity_soc.png
    └── port_terminal_dwell_sensitivity_soc.png
```
# 3. Data Sources

The analysis uses representative truck operational profiles and parameters related to vehicle energy use and charging.

# 4. Reproduction Guide

**Requirements**

To reproduce the analysis, the following are required:
- Python 3.12
- Intalled Python packages:
  - `numpy`
  - `pandas`
  - `matplotlib`
  - `seaborn`
  - `ipython`
 
Install the notebook environment, for example, from a terminal in the repository root:

```bash
python -m pip install numpy pandas matplotlib seaborn ipython jupyter ipykernel
```
 
**General setup**

1. Download or clone the GitHub repository.
2. Open the repository folder in Jupyter Notebook or Visual Studio Code.
3. Install the required packages listed above.
4. Open **`Final_code.ipynb`** (the filename of the supplied final notebook).
5. Select a Python kernel with the required packages installed.
6. Run **all cells from top to bottom**. Classes, scenario objects, simulation functions, result tables, and plotting code are defined in preceding cells and must be executed in order.

**Key parameters**

| Parameter | Base value | Basis |
|---|---:|---|
| Nominal battery capacity | 540 kWh | Smart Freight Centre (2024) |
| Energy Consumption | 1.7 kWh/km | Smart Freight Centre (2024)  |
| Average driving speed | 60 km/h | Smart Freight Centre (2024) |
| Destination dwell time | 0.75 hours (45 minutes) | Smart Freight Centre (2024) |
| Intermediate dwell time | 0.75 hours (45 minutes) | Model assumption |
| Duty start | 09:00 | Smart Freight Centre (2024) and model assumption |
| One-way distance: Short / Medium / Long | 30 / 92.5 / 187.5 km | Midpoints of Smart Freight Centre (2024) distance bands |
| Round trips: Short / Medium / Long | 3 / 2 / 1 | Smart Freight Centre (2024) |
| Minimum / maximum / initial SOC | 10% / 90% / 90% | Model assumption |
| Charging efficiency | 90% | Model assumption |
| Depot charging power | 150 kW | Model assumption informed by Smart Freight Centre (2024) |
| Opportunity charging power | 0 / 180 / 360 kW | Tested alternatives; PoR/SFC references |
| Opportunity charger availability | All eligible dwell events | Model assumption |
| Final return | Duty ends; no additional terminal dwell | Model assumption |
| Next departure | 24 hours after duty start | Model assumption |

**Execution order within the notebook**

The notebook contains the following sections, which are executed sequentially:

1. **Library imports:** load dependencies and configure plots.
2. **Class definitions:** create `Electric_Truck`, `Operational_Profile`, and `Charging_Option`.
3. **Baseline scenario setup:** instantiate the truck, the three driving profiles, and the three charging options.
4. **Duty-event construction:** create chronological driving and dwell events.
5. **Simulation algorithm:** model driving energy demand, charging during eligible dwell periods, SOC limits, and overnight depot recovery.
6. **Run base scenarios:** calculate the nine scenarios; create tables for duty feasibility and repeatable daily feasibility.
7. **Baseline visualizations:** export feasibility matrices, battery SOC trajectories, and minimum SOC bar chart.
8. **Consumption sensitivity:** vary consumption rates, rerun scenarios, and plot minimum SOC.
9. **Dwell-time sensitivity:** vary destination and port-terminal dwell durations separately, rerun scenarios, and plot minimum SOC.

# 5. Authors and contributions


