# Berlin-Urban-Energy-Simulation
Urban energy system simulator integrating PV, battery storage, heat pump sector coupling, time-of-use pricing, and CO₂ accounting. Models hourly electricity and heating demand, storage dynamics, and grid interaction, comparing rule-based and optimization dispatch to evaluate cost, emissions, and peak load performance.


# Urban Energy System Simulation Framework (Berlin-Style Microgrid)

## Introduction  
This project presents a modular simulation framework for modeling an urban-scale energy system representative of residential or small block-level infrastructure in Berlin. The framework integrates:

- **Electric subsystem**: base electricity demand, PV generation, battery storage, and grid exchange  
- **Heating subsystem**: space heating demand, heat pump electricity-to-heat conversion, and a thermal storage tank  
- **External signals**: time-of-use electricity tariffs and time-varying grid CO₂ intensity  

The simulation is designed to support graduate-level sustainability and energy system analysis by combining **physics-based state dynamics**, **control logic**, and **systems evaluation metrics** (cost, emissions, peak demand, carbon intensity). It reflects real-world decarbonization challenges in urban infrastructure systems.

---

## Aim of the Project  
The primary objectives of this project were:

- To implement an hourly **PV + battery + grid** microgrid simulator for a full year.
- To incorporate **TOU electricity pricing** and quantify cost impacts.
- To model **sector coupling** via a **heat pump + thermal storage tank**, linking electricity and heat demand.
- To compute **CO₂ emissions** based on grid electricity imports and time-varying carbon intensity.
- To compare a **rule-based control strategy** with an **optimization-based dispatch (linear programming)**.
- To evaluate system performance using quantitative KPIs and visual analysis.

---

## Problems Addressed  

### 1. Multi-Energy Coupling (Electricity ↔ Heat)  
Urban decarbonization requires integration between electricity and heating sectors.  
This model captures:

- Heat demand satisfaction via a thermal storage tank.
- Heat pump electricity consumption feeding back into electric demand.
- Interdependence between renewable availability and heating operation.

---

### 2. Storage Dynamics and Operational Constraints  

**Battery constraints:**
- Maximum charge/discharge power  
- Charge/discharge efficiencies  
- State-of-charge bounds  

**Thermal tank constraints:**
- Energy capacity limits  
- Minimum energy constraint  
- Standby heat loss  

This ensures physically consistent system behavior.

---

### 3. Economic Dispatch Under Variable Tariffs  

- Hourly import prices follow a time-of-use structure.
- Export revenue is modeled via feed-in tariff.
- Annual cost and peak import are quantified.
- Optimization provides a cost-minimizing benchmark.

---

### 4. Carbon Accounting in Time-Series Systems  

- Grid CO₂ intensity varies hourly (synthetic seasonal + diurnal model).
- Emissions are computed only from grid imports.
- Both instantaneous and cumulative CO₂ emissions are tracked.
- Annual carbon intensity metrics are calculated.

---

### 5. Rule-Based Control vs Optimization  

Two dispatch strategies are implemented:

**Rule-Based Strategy**
- PV → load → battery → export
- Deficit → battery → grid
- Heat demand → tank → heat pump

**Optimization Strategy (Linear Programming)**
- Minimizes cost (optionally includes CO₂ penalty)
- Respects all storage and system dynamics constraints
- Solves daily rolling-horizon problems
- Provides theoretical best-case system operation

---

## Methodology  

### 1. System Model Overview  

The simulation operates on an hourly time grid for one full year.

At each timestep:

**Electric balance:**
- PV generation
- Base electric load
- Heat pump load
- Battery charge/discharge
- Grid import/export

**Thermal balance:**
- Heat demand
- Heat pump heat production
- Tank charging/discharging
- Tank standby losses

---

### 2. PV and Load Modeling  

Synthetic Berlin-style profiles include:

- Morning and evening electricity peaks
- Seasonal solar variation
- Diurnal PV curve
- Weekend load adjustment
- Random noise for realism

These profiles provide sufficient dynamic variability for control and optimization analysis.

---

### 3. Heat Pump + Thermal Tank Model  

**Heat demand model:**
\[
Q_{heat} = UA \cdot (T_{indoor} - T_{outdoor})
\]

**Heat pump heat production:**
\[
Q_{hp} = COP(T_{out}) \cdot P_{hp}
\]

**Tank energy update:**
- Increase from stored heat
- Decrease from heating demand
- Reduction from standby loss

COP is modeled as a clipped linear function of outdoor temperature.

---

### 4. Battery Model  

State-of-charge evolution:

- Charging:  
\[
SOC_{t+1} = SOC_t + \eta_{ch} P_{ch} \Delta t
\]

- Discharging:  
\[
SOC_{t+1} = SOC_t - \frac{P_{dis} \Delta t}{\eta_{dis}}
\]

All constraints are enforced each timestep.

---

### 5. Economic Model  

Hourly net cost:

\[
Cost_t = Import_t \cdot Price_{import,t}
          - Export_t \cdot Price_{export,t}
\]

Annual cost and peak grid import are evaluated.

---

### 6. CO₂ Accounting  

CO₂ emissions are computed from grid imports:

\[
CO₂_t = Import_{kWh,t} \cdot \frac{CO₂_{grid,t}}{1000}
\]

The model produces:

- Hourly emissions  
- Total annual emissions  
- System CO₂ intensity  
- Cumulative CO₂ emissions curve  

This enables environmental performance assessment.

---

## Simulation Outputs  

The framework generates:

- Weekly power flow plots  
- Battery SOC and tank energy plots  
- Annual KPI summary  
- Cumulative CO₂ emissions over the year  
- Comparison between rule-based and optimized dispatch  

These outputs enable both physical validation and sustainability evaluation.

---

## Tools Used  

- Python  
- NumPy  
- Pandas  
- Matplotlib  
- SciPy (for linear programming optimization)

Conceptual foundations include:

- Energy balance modeling  
- Storage dynamics  
- Heat pump thermodynamics  
- Time-series economic analysis  
- Carbon intensity modeling  
- Linear programming optimization  

---

## Analysis  

### Rule-Based Operation  

- Provides realistic and stable operation.
- Demonstrates PV self-consumption enhancement through battery.
- Shows heat pump influence on electric peak demand.
- Produces measurable cumulative CO₂ emissions over the year.

---

### Optimization-Based Operation  

- Reduces annual cost under TOU pricing.
- Lowers peak grid import.
- Potentially reduces CO₂ when aligned with low-intensity hours.
- Demonstrates value of coordinated storage dispatch.

---

## Findings  

- Sector coupling significantly alters electricity demand patterns.
- Thermal storage enables shifting heat pump operation.
- Battery storage increases PV self-consumption.
- Time-varying carbon intensity creates emissions concentration in winter and evening peaks.
- Optimization defines a performance upper bound for system efficiency.
- The model remains numerically stable over full-year simulation.

---

## Future Work  

Potential extensions include:

- Integration of real Berlin load and PV datasets.
- Incorporation of real German grid CO₂ intensity signals.
- Electric vehicle charging integration.
- Demand response and flexible load modeling.
- Multi-objective optimization (cost vs CO₂ Pareto analysis).
- Monte Carlo uncertainty analysis.
- Building RC thermal models (higher fidelity heat dynamics).
- Peak shaving objective functions.
- Capacity sizing optimization (PV, battery, tank).

---

## Academic Context  

This project demonstrates the structured integration of:

- Energy systems engineering  
- Thermodynamic conversion modeling  
- Storage state dynamics  
- Techno-economic analysis  
- Carbon accounting methodologies  
- Constrained optimization techniques  

The work illustrates the ability to translate physical and economic system models into stable computational implementations and evaluate them using quantitative performance metrics. It provides a foundation for advanced study in sustainable energy systems, environmental engineering, and energy transition modeling.

