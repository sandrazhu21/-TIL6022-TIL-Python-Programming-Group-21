# TIL6022 - TIL Python Programming Project Group 21

Members: Andrea De Felice, Magdalini Karataraki, Stella Nestori, Fernando Quiros, Yiying Sandra Zhu Zhou

Student numbers: 6591973, 6794629, 6811523, 5237076, 6615201

# Research Objective

The objective of this project is to determine which representative truck duties serving the Port of Rotterdam can be completed with depot charging alone and which require additional opportunity charging during scheduled dwell periods. A common set of vehicle specifications is applied to the three representative operational profiles, short, medium, and long-distance to examine how differences in travel distance, number of round trips, and charging opportunities affect feasibility. 

The analysis considers two requirements. The first is duty feasibility: whether the truck can complete its scheduled driving and dwell events without its battery falling below the minimum state of charge. The second is repeatable daily feasibility: whether the truck can also restore its initial battery level before the next day’s departure. We compare depot charging alone with two configurations that additionally allow opportunity charging at 180 kW and 360 kW. 

The analysis also examines how changes in energy consumption, destination dwell time, and port-terminal dwell time affect the results. This allows the sensitivity of the simulated configurations to selected operating assumptions to be assessed. The aim is to identify the conditions under which each charging configuration can support individual truck duties and provide a starting point for more detailed charging infrastructure studies. 

**Research questions**

*Main research question*

For representative short-, medium-, and long-distance truck duties serving the Port of Rotterdam, which can be completed with overnight depot charging alone, and which require additional opportunity charging during scheduled dwell periods at origin and destination? 

*Sub questions*

What are the daily driving energy requirements of the representative short-, medium-, and long-distance truck duties? 

Which combinations of operating profile and charging configuration allow the truck to complete its daily duty without falling below the minimum state of charge? 

For the configurations that allow completion of the daily duty, is the remaining time before the next departure sufficient to restore the battery to its initial state of charge using depot charging? 

How sensitive is duty feasibility to changes in energy consumption, destination dwell time, and intermediate port-terminal dwell time? 

# Contribution Statement

*Be specific. Some of the tasks can be coding (expect everyone to do this), background research, conceptualisation, visualisation, data analysis, data modelling*

**Author 1**:

**Author 2**:

**Author 3**:

**Author 4**:

**Author 5**:

# Data Used

Starting with the data collection phase, a preliminary assessment of electric truck operations serving the Port of Rotterdam was conducted and some representative values have been collected from Smart Freight Centre (2024) to have a realistic set of numbers to define the simulated trucks and charging configurations. They act as the baseline values for the model. Additional assumptions were established to define the operating schedules, battery limits, and charging conditions within the scope of the model.

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

# Data Pipeline

1. Data collection 

    a. truck-specific assumptions (battery capacity, electricity consumption) 

    b. operational profile parameters (route distances, trips per day) 

    c. charging configurations (charging power) 

2. Data preparation: format all input parameters into required structures 

3. Computation:  

    a. Create relevant functions such as decreasing battery over trip execution, estimating time needed to complete charging etc. 

    b. Create events of truck duties 

    c. Process each truck profile under different scenarios defined above 

4. Simulation: keeping track of the energy consumed and available 

5. Evaluation: assessing feasibility and computing performance metrics 

6. Visualisation: SOC trajectories, feasibility outputs, sensitivity plots 
