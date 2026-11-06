# TIL6022 - TIL Python Programming Project Group 21

Members: Andrea De Felice, Magdalini Karataraki, Stella Nestori, Fernando Quiros, Yiying Sandra Zhu Zhou

# Research Objective

The objective of this project is to determine which representative truck duties serving the Port of Rotterdam can be completed with depot charging alone and which require additional opportunity charging during scheduled dwell periods. A common set of vehicle specifications is applied to the three representative operational profiles, short, medium, and long-distance to examine how differences in travel distance, number of round trips, and charging opportunities affect feasibility. 

The analysis considers two requirements. The first is duty feasibility: whether the truck can complete its scheduled driving and dwell events without its battery falling below the minimum state of charge. The second is repeatable daily feasibility: whether the truck can also restore its initial battery level before the next day’s departure. We compare depot charging alone with two configurations that additionally allow opportunity charging at 180 kW and 360 kW. 

# Reseach Questions

**Main Research Question**

For representative short-, medium-, and long-distance truck duties serving the Port of Rotterdam, which can be completed with overnight depot charging alone, and which require additional opportunity charging during scheduled dwell periods at origin and destination? 

To answer the main research question, the following sub questions are addressed: 

**Sub questions**

What are the daily driving energy requirements of the representative short-, medium-, and long-distance truck duties? 

Which combinations of operating profile and charging configuration allow the truck to complete its daily duty without falling below the minimum state of charge? 

For the configurations that allow completion of the daily duty, is the remaining time before the next departure sufficient to restore the battery to its initial state of charge using depot charging? 

How sensitive is duty feasibility to changes in energy consumption, destination dwell time, and intermediate port-terminal dwell time?

# Methodology

**Data Analysis Pipeline**

To determine which truck duties can be completed with overnight depot charging alone and which require opportunity charging, we developed a deterministic, event-based simulation of one working day for representative short, medium, and long-haul electric trucks serving the Port of Rotterdam (PoR). The model tracks the truck's battery through a chronological sequence of driving and dwell events, checking after each event whether the minimum State of Charge (SOC) requirement is respected. It subsequently tests whether the vehicle can recover its energy overnight at the depot before the next duty. By evaluating the three operational profiles in correlation with three alternative charging configurations, the framework establishes nine baseline scenarios to compare different route requirements and charging strategies. 

The simulation workflow is initialized by gathering preliminary electric truck operational values from the Smart Freight Centre (2024), defining a nominal battery capacity of 540 kWh, an energy consumption rate of 1.7 kWh/km, and a charging efficiency of 90%. Operational profile parameters establish route distances and trips per day across Short (30 km one-way, 3 trips), Medium (92.5 km one-way, 2 trips), and Long (187.5 km one-way, 1 trip) profiles. The profiles are set as the mean between the distances presented in the report (for short:0-60km, medium: 60-125km, and long:125-250km respectively). 

Charging configurations set depot charging power to 150 kW (SFC, 2024) alongside opportunity charging alternatives of 180, and 360 kW based on the available chargers at the selected location of Waalhaven facility (PoR, n.d.). Through Python classes representing electric trucks, operational profiles, and charging options, the model formats input parameters into structured time-step event lists encompassing outbound drives, destination dwells, return drives, and port-terminal dwells. 

During simulation execution, energy consumption is tracked as distance multiplied by consumption during driving legs, while battery headroom and charging power limits dictate energy intake during eligible dwell periods based on the following mathematical relations: 

battery headroom=maximum stored energy−current stored energy 

actual grid energy=min (power * dwell hours, headroom/ efficiency) 

battery energy received=actual grid energy* efficiency 

The feasibility evaluation assesses whether duties maintain SOC above the 10% safety floor and whether sufficient off-duty hours remain for a full recharge to the upper operational limit (90% absolute SOC / 100% of the usable window) within a strict 24-hour window. 

Executing the simulation under the baseline parameter set yields distinct feasibility profiles for each haul type. Short-haul duties consume 306 kWh of driving energy, remaining well within the 432 kWh usable battery window. Depot-only operation completes the shift ending at 33.3% SOC and requires approximately 2.27 hours of depot recharging, fitting comfortably within the 17.25-hour off-duty window, rendering opportunity charging unnecessary.  

Conversely, medium-haul duties require 629 kWh; under a depot-only setup, the first SOC violation occurs on the second outbound drive. However, with 180 kW opportunity charging available across three dwell periods, the battery receives 364.5 kWh, maintaining a minimum operational SOC of approximately 41.0%.  

Lastly, long-haul duties require 637.5 kWh, where a single 45-minute dwell supplied at 180 kW yields 121.5 kWh—insufficient to preserve the safety reserve on the return leg. Scaling opportunity charging to 360 kW supplies 243 kWh during the dwell, securing a minimum SOC of approximately 16.9% and enabling a full depot recharge in about 2.92 hours within a 17-hour off-duty window. 

Battery capacity bounds energy uptake; for instance, the short route accepts a cumulative maximum of only 255 kWh of opportunity energy across all five dwells regardless of whether 180 kW or 360 kW chargers are installed, as additional charger power yields no further energy gains once the 90% SOC ceiling is reached. While larger batteries, lower consumption rates, and higher charger power expand technical feasibility, total daily energy balance alone does not guarantee success, because certain configurations fail on intermediate driving legs before a later opportunity charger can be reached. 

These scenario results reflect deterministic outcomes under stated assumptions rather than empirical service-performance claims. Charging power, energy consumption per kilometer, and vehicle payloads are treated as constant and independent of external factors such as weather, topography, or traffic speeds. Furthermore, opportunity chargers are assumed to be continuously available at every destination and port-terminal dwell without queuing delays, detours, or extra stopping overhead, with all vehicles initiating their shifts at 90% SOC from a centralized depot at the Port of Rotterdam on a strict, repeatable 24-hour cycle.

**Data Used**

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

# Results

*Energy requirements*

The simulation of energy requirements of representative short, medium, and long-distance truck services in the PoR, shows significant differences in daily energy requirements. With the operational parameters given, the short distance profile (180 km) requires 306 kWh, in contrast, the medium distance (370 km) requires 629 kWh, while the long-distance duty (375 km) requires 637.5 kWh. 

The short-distance duty requires less energy than 432 kWh of usable battery energy within the operating SOC range of 10% to 90%, making depot only charging feasible. In contrast, medium, and long-distance duties require 629 kWh and 637.5 kWh, respectively, exceeding the nominal battery capacity (540 kWh) and requiring opportunity charging during daily operations. 

*Feasible configurations*

![Figure 01](report_outputs/Figure%2001.png)
Figure 01. En-route feasibility by operational profile and charging modality 

![Figure 02](report_outputs/Figure%2002.png)
Figure 02. Repeatable daily feasibility with overnight depot charging 

Figure 01 and Figure 02 present the en-route and repeatable feasibility across the three operational profiles and charging configurations. The short-distance duty is feasible under all configurations, whereas the medium-distance duty requires at least 180 kW opportunity charging, and the long-distance duty requires 360 kW opportunity charging. Both matrices show identical feasibility results, indicating that all configurations capable of completing the daily duty can also recover their initial SOC through overnight depot charging before the next duty. Therefore, overnight recharging does not introduce an additional feasibility constraint under baseline conditions. 

![Figure 03](report_outputs/Figure%2003.png)
Figure 03. Minimum recorded SOC by profile and charging configuration 

Figure 03 compares the minimum recorded SOC for each operational profile and charging configuration, using the feasibility threshold. The short distance duty maintains a minimum SOC of 33.3% with depot-only charging, increasing to approximately 80% with opportunity charging. For the medium-distance duty, the minimum SOC increases to 41.0% with 180 kW charging, and to 60.9% with 360 kW charging. In contrast, the long-distance duty remains below the minimum threshold under depot-only and 180 kW charging, while 360 kW charging maintains a minimum SOC of roughly 16.9% providing a limited margin above the threshold, just 6.9 percentage points above the threshold. 

![Figure 04](report_outputs/Figure%2004.png)
Figure 04. SOC trajectories by daily operations 

Figure 04 presents the SOC trajectories throughout the daily operations. Short distance duties remain above the minimum threshold even without opportunity charging. For medium-distance duties, depot-only charging becomes insufficient during the second-round trip, while intermediate charging maintains operational feasibility. Long-distance duties depend on the single destination charging opportunity, where only the 360 kW configuration provides sufficient energy to complete the return trip. 

*Sensitivity analysis*

The sensitivity analysis evaluates how changes in energy consumption, destination dwell time, and port terminal dwell time affect the minimum SOC threshold and operational feasibility. A total of 180 simulations were performed, varying one parameter at a time across three operational profiles and three charging configurations. These include 72 simulations for energy consumption and 54 simulations for each dwell time parameter (destination and port-terminal), while the remaining parameters were maintained at their baseline values. 

The tested values were selected within predefined minimum and maximum around the baseline assumptions, using equally spaced intervals to assess changes in operational feasibility. 

| Parameter | Values tested |
|---|---:|
| Energy Consumption | 0.500, 0.929, 1.357, 1.786, 2.214, 2.643, 3.071, 3.500 kWh/km |
| Destination dwell time | 0.25, 0.50, 0.75, 1.00, 1.25 and 1.50 hours |
| Port terminal dwell time  | 0.25, 0.50, 0.75, 1.00, 1.25 and 1.50 hours |

![Figure 05](report_outputs/Figure%2005.png)
Figure 05. Sensitivity of minimum battery SOC to energy consumption 

Figure 05 shows the relationship between energy consumption (kWh/km) and minimum SOC (%) for each operational profile. Higher energy consumption generally reduces the minimum SOC, particularly for medium and long-distance duties. The short-distance duty remains feasible with opportunity charging throughout the tested range, while depot-only charging becomes infeasible at approximately 2.6 kWh/km. For the medium-distance duty, the 180 kW configurations fall below the minimum threshold at approximately 2.2 kWh/km, whereas 360 kW charging maintains feasibility up to approximately 3.1 kWh/km. The long-distance duty is more sensitive, with 360 kW charging falling below the threshold at approximately 2.21 kWh/km. 

Additionally, for the medium-distance profile, some fluctuations in minimum SOC occur because the simulation stops when the SOC falls below the threshold. Higher energy consumption may therefore cause earlier termination and a higher recorded minimum SOC, rather than an actual improvement in battery performance. 

![Figure 06](report_outputs/Figure%2006.png)
Figure 06. Sensitivity of minimum battery SOC to destination dwell time. 

Figure 06 illustrates how minimum SOC changes as destination dwell time increases from 0.25 to 1.5 hours. For short-distance duties, the minimum SOC remains above 30% with depot only charging and approximately 80% with opportunity charging. For medium-distance duties, 360 kW charging reaches approximately 61% minimum SOC with 0.5-hour dwell, while 180 kW charging increases from approximately 11% to 54.3% as dwell time increases. For long-distance duties, 360 kW charging becomes feasible with approximately 0.75 hour of destination dwell, whereas 180 kW charging requires approximately 1.5 hours. 

![Figure 07](report_outputs/Figure%2007.png)
Figure 07. Sensitivity of minimum battery SOC to port terminal dwell time. 

Figure 07 shows the impact of increasing port terminal dwell time from 0.25 to 1.5 hours. Short distance duties remain feasible under all charging configurations, with no significant improvement from longer dwell periods. For medium-distance duties, the minimum SOC with 180 kW charging increases from approximately 26% to 54.3%, while 360 kW charging reaches approximately 60.9% with a 0.50-hour dwell. In contrast, the long-distance duty shows no changes in minimum SOC because its operational schedule includes only one round a trip without intermediate port terminal charging opportunities. 

# Conclusion

The simulation results indicate that the energy requirements of representative truck duties differ significantly across operational profiles. Short-distance duties can be supported by the available battery energy, whereas medium and long-distance duties exceed the usable battery capacity, making additional charging necessary under the baseline assumptions. 

Regarding duty feasibility, overnight depot charging is sufficient for short-distance profile, while medium-distance profile is feasible with 180 kW opportunity charging and long-distance profile is feasible only with 360 kW. These findings suggest that the charging requirements are influenced not only by energy demand, but also by the availability of charging opportunities throughout the daily operation. 

Regarding repeatable daily feasibility, all configurations capable of completing their duties can also restore the initial % SOC before the next departure. Therefore, overnight depot charging did not introduce an additional constraint in the baseline simulations. 

The sensitivity analysis indicates that operational feasibility is dependent on the parameters used (energy consumption and the availability of charging opportunities). Longer dwell periods can improve feasibility, but their effectiveness depends on the operational schedule and charging power. Therefore, the feasibility of electric truck operations cannot be assessed from distance alone within the simulated profiles, since trucks with similar daily energy requirements may need different charging configurations depending on the frequency and duration of available charging opportunities. 

Overall, the study provides a preliminary assessment of charging feasibility for three representative freight operations serving the PoR, based on selected technical and operational assumptions. Within the simulated conditions, depot-only charging is sufficient for the short-distance duty, whereas medium and long-distance profiles require additional opportunity to charge at different power levels. The sensitivity analysis provides relevant information on how changes in energy consumption and dwell times affect operational feasibility within the scope of the model. 

# Discussion

The findings provide a basis for extending the current simulation towards a broader assessment of charging infrastructure in the PoR. Future developments could incorporate simultaneous truck movements, charging demand across different terminals, charger availability, and operational constraints. By integrating these factors, the model could move beyond individual duty feasibility to estimate the number and distribution of charging points required to support electric freight operations at the port network level. 

Several limitations should be considered when interpreting these findings. First, the simulation used three representative operational profiles with fixed distances, travel speeds, and schedules. Although these profiles provide a basis for comparison, they do not capture the full diversity of truck operations serving the PoR, including variations in traffic conditions, vehicle loads, and daily travel patterns. 

Second, the model assumes constant energy consumption, charging power, and charging efficiency. It also excludes real conditions as unavailable chargers during scheduled dwell times, queues, or waiting times. In practice, variations in energy consumption, charging performance, and infrastructure availability could reduce energy recovered and affect operational feasibility.  

Furthermore, the sensitivity analysis is limited to test truck specifications and representative operating conditions; therefore, it evaluates individual truck duties rather than simultaneous charging requirements of multiple vehicles as determining for example, the number of chargers required to serve the overall freight network. 

# Contribution Statement

**Author 1**: Andrea De Felice contributed to background research by drafting the initial context, objective and research question, and to python modelling and simulation.

**Author 2**: Fernando Quiros contributed to conceptualisation, data analysis and writing the Read me file.

**Author 3**: Magdalini Karataraki contributed to reviewing the final report and read me file, visualisation and data analysis.

**Author 4**: Stella Nestori contributed to background research by drafting the initial project context, objective and research questions.

**Author 5**: Sandra Zhu Zhou contributed to preparing the final report and Read me file, and data modelling, visualization and sensitivity analysis.

# References
European Comission. (2026). Heavy-duty vehicles: Council adopts targeted flexibility for manufacturers to comply with CO2 targets (https://www.consilium.europa.eu/en/press/press-releases/2026/03/30/heavy-duty-vehicles-council-adopts-targeted-flexibility-for-manufacturers-to-comply-with-co2-targets/) 

European Environment Agency. (2026, June 27). Transport and mobility. https://www.eea.europa.eu/en/topics/in-depth/transport-and-mobility 

Port of Rotterdam (n.d.). Charging station for electric trucks (https://www.portofrotterdam.com/en/port-future/energy-transition/making-logistics-chains-more-sustainable/charging-station-for).  

Smart Freight Centre (2024). Preliminary assessment of e-truck operational and economic viability: Drayage operations from and to the Port of Rotterdam (https://smartfreightcentre.org/news/13311296).  

Volvo Trucks. (2021, November 29). How a good charging strategy can extend an electric truck’s range. VOLVO Group. https://www.volvotrucks.com/en-en/news-stories/insights/articles/2021/nov/How-a-good-charging-strategy-can-extend-an-electric-trucks-range.html 

# AI Statement

We acknowledge the use of AI in preparing this submission. We used it for the following purposes: Refining language (grammar, vocabulary, clarity, and consistency in the report, ...). 

Andrea De Felice used ChatGPT to assist in refining language in report writing and evaluating code structure and debugging errors within the Python simulation pipeline.

Fernando Quiros used ChatGPT to assist in refining language, and academic writing in the Results, Discussion, and Conclusions section. 

Magdalini Karataraki used Gemini to assist with language refinement, grammar correction, and syntax checking within the report's text, as well as evaluating code structure and debugging errors within the Python simulation pipeline. 

Stella Nestori used ChatGPT to assist in refining language in report writing. 

Sandra Zhu Zhou used ChatGPT to assist in refining language in report writing, as well as evaluating code structure and debugging errors within the Python simulation pipeline.

We confirm that we have critically reviewed and verified all AI-assisted content, that the submitted work reflects our own understanding, and that we take full responsibility for its accuracy and originality



