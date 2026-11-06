# Electrification of Road Freight Transport in the Port of Rotterdam: Assessment of Charging Feasibility

**Date**: November 2026
**Course:** TIL6022 - TIL Python Programming
**Group:** Group 21
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
    ├── consumption_sensitivity_soc.png
    ├── destination_dwell_sensitivity_soc.png
    ├── duty_feasibility_matrix.png
    ├── feasibility_results.csv
    ├── matrix.png
    ├── min_soc_bar.png
    ├── port_terminal_dwell_sensitivity_soc.png
    ├── port_terminal_extreme_scenario_hourly_term_grid_electricity.png
    ├── port_terminal_extreme_scenario_hourly_visit.png
    ├── repeatability_matrix.png
    ├── speed_intensity_profile.png
    ├── trajectories.png
    ├── port_terminal_extreme_scenario_hourly_visit.png.png
    ├── port_terminal_extreme_scenario_hourly_term_grid_electricity.png
```
# 3. Data Sources and Scenario Assumptions


The analysis uses representative truck operational profiles and parameters related to vehicle energy use and charging. NDW DEXTER Traffic & Flow Data: Historical and empirical traffic characteristics (including traffic volumes, flow profiles, and speed variability) were retrieved via the Nationaal Dataportaal Wegverkeer (NDW) DEXTER portal. This dataset provides high-resolution monitoring data collected from road sensors across the Dutch road network, which was utilized to simulate realistic arrival clustering and traffic flow dynamics for the port-drayage operations serving the Port of Rotterdam.




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
10. **Peak-hour arrival sensitivity (DEXTER):** integrate empirical hourly flow profiles from the NDW DEXTER dataset to test operational feasibility under dynamic peak-hour congestion and arrival clustering.
11. **Grid impact & terminal electricity demand:** evaluate peak power draw across multi-truck arrival scenarios to model hourly grid load and localized charging infrastructure constraints at port terminals.

# 5. Authors and contributions

| Group member | Contribution |
|---|---|
| Andrea de Felice | Drafted initial project context, objective and research questions and contributed to Python code for the Simulation and Modelling part  |
| Fernando Quiros |  Wrote Results, Conclusions, Discussion, and Read me file.  |
| Magdalini Karataraki | Wrote and reviewed Final report and Read me, contributed to Python code for review, visualization and data analysis for worst case scenario as sensitivity  |
| Stella Nestori | Drafted initial project context and research questions  |
| Sandra Zhu Zhou  | Wrote and reviewed Final report and Read me, contributed to Python code for review, visualization and sensitivity analysis |

# 6. References

European Comission. (2026). Heavy-duty vehicles: Council adopts targeted flexibility for manufacturers to comply with CO2 targets https://www.consilium.europa.eu/en/press/press-releases/2026/03/30/heavy-duty-vehicles-council-adopts-targeted-flexibility-for-manufacturers-to-comply-with-co2-targets/ 

European Environment Agency. (2026, June 27). Transport and mobility. https://www.eea.europa.eu/en/topics/in-depth/transport-and-mobility 

Port of Rotterdam (n.d.). Charging station for electric trucks https://www.portofrotterdam.com/en/port-future/energy-transition/making-logistics-chains-more-sustainable/charging-station-for 

Smart Freight Centre (2024). Preliminary assessment of e-truck operational and economic viability: Drayage operations from and to the Port of Rotterdam https://smartfreightcentre.org/news/13311296 

Volvo Trucks. (2021, November 29). How a good charging strategy can extend an electric truck’s range. VOLVO Group. https://www.volvotrucks.com/en-en/news-stories/insights/articles/2021/nov/How-a-good-charging-strategy-can-extend-an-electric-trucks-range.html 

European Commission. (2026). Lorries, buses and coaches. https://climate.ec.europa.eu/areas-action/transport-decarbonisation/road-transport/lorries-buses-and-coaches_en?utm_source=chatgpt.com 

Port of Rotterdam Authority. (2024, January 26). First charging station for electric trucks opened in the port of Rotterdam. https://www.portofrotterdam.com/en/news-and-press-releases/first-charging-station-for-electric-trucks-opened-in-the-port-of-rotterdam?utm_source=chatgpt.com

