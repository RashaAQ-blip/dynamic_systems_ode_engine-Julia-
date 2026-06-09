# Dynamic Time-Series Systems & Predictive ODE Engines

A high-fidelity computational repository implemented natively in Julia. This suite constructs numerical initial-value problem (IVP) solver architectures to simulate continuous mathematical systems, minimize localized truncation drift, and track numerical stability across explosive, non-linear dynamic time-series domains.

## Tech Stack & Core Libraries
* **Language:** Julia v1.x
* **PyPlot:** Engine for data-dense phase portrait mapping, analytical curves, and multi-step convergence visualization.
* **Base Mathematics:** Multi-step vector history tracking, linear approximations, and Runge-Kutta fourth-order initializations.

---

## System Architecture & Core Insights

### 1. Single-Step Linear Time-Series Integration (Euler's Framework)
* **Implementation:** Models a continuous derivative landscape across varying discrete time-step partitions ($h = 1.0, 0.5, 0.25, 0.125$) against the system's analytical exact curve on the interval $[0, 3]$.
* **Core Insight:** Demonstrates the behavioral constraints of first-order convergence. When step sizes are large ($h = 1.0$), localized linear tangent approximations fail to capture underlying curvature, resulting in accumulated tracking error. Systematically narrowing the step partition down to $h = 0.125$ reduces local truncation error, forcing the discrete approximation to converge tightly onto the true physical state.

### 2. High-Order Multi-Step Predictor-Corrector Engine (AB4-AM3)
* **Implementation:** Combines an explicit 4-step Adams-Bashforth projection model with an implicit 3-step Adams-Moulton error-tightening pass at a highly efficient fixed step size ($h = 0.05$) across the interval $[0, 5]$.
* **Core Insight:** Solves the challenge of rapid, non-linear velocity acceleration. Standard explicit single-step models require infinitesimally small steps or suffer catastrophic divergence when processing explosive exponential systems (such as the $3e^{t^2/2}$ term in $y' = ty + t^3$). This dual-stage pipeline leverages historical multi-step trajectory memory to explicitly project the next state, then instantly runs an implicit constraint verification to refine the value. The engine remains strictly stable, completely eliminating numerical phase lag and structural drift near the boundary vector ($t = 5$).

---

## Repository File Checklist
* **`dynamic_systems_ode_engine.ipynb`**: The interactive Jupyter Notebook containing fully documented implementation blocks, terminal plotting configurations, and inline time-series engineering interpretations.
* **`README.md`**: Executive project layout summary and core mathematical takeaways.

## Environment Setup & Execution
To initialize the dependencies and launch this simulation project locally, clone this repository, open your terminal, and execute:

```bash
# Install the required plotting backend via the Julia package manager
julia -e "using Pkg; Pkg.add(\"PyPlot\")"

# Launch your local Jupyter environment
jupyter notebook
