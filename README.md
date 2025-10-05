# Two-Stage Stochastic MILP Model for Kidney Exchange under Crossmatch Uncertainty

This project replicates the **two-stage stochastic Mixed-Integer Linear Programming (MILP)** model proposed in a published article on **probabilistic kidney exchange**.  
Developed as a **course project for Healthcare Systems Engineering (Spring 2025)**, it focuses on replicating, coding, and solving the model in **Python (Pyomo)** with **Sample Average Approximation (SAA)** for handling uncertainty in crossmatch outcomes.

## Overview

Kidney exchange programs aim to match incompatible donor–recipient pairs through chains and cycles, maximizing successful transplants.  
However, the outcome of each potential match is uncertain due to crossmatch test results.  
This project implements a **two-stage stochastic optimization model** where:

- **Stage 1:** Decides which donor–recipient pairs to test (under a limited testing budget)  
- **Stage 2:** Determines feasible transplants after the test outcomes are revealed  

The model’s goal is to **maximize the expected number of realized transplants** while adhering to clinical and logistical constraints.

## Model Summary

**Type:** Two-Stage Stochastic MILP (NP-hard)  
**Approach:** Sample Average Approximation (SAA)  

### First Stage
- Decision: Which arcs (potential transplants) to test  
- Constraint: Crossmatch test budget  
- Objective contribution: None (anticipates future outcomes)

### Second Stage (Recourse)
- Decision: Select feasible transplants under each scenario  
- Constraints:
  - Each patient can appear in at most one transplant  
  - A transplant can occur only if its test was selected in the first stage  
- Objective: Expected number of realized transplants + small tie-breaker term  

## Implementation Details

- **Language:** Python  
- **Modeling Library:** Pyomo  
- **Solver:** Gurobi / GLPK (depending on setup)  
- **Scenario Handling:** Sample Average Approximation (SAA) with uniform weights  

### Code Structure
- Random instance generation (graphs, success probabilities, budgets)  
- Cycle and chain construction (valid exchange structures)  
- Scenario generation for probabilistic outcomes  
- Model formulation:
  - Binary first-stage test selection variables  
  - Scenario-based recourse variables  
  - Linking constraints between stages  
- Solving and collecting results (expected transplants, tested arcs, runtime)

## Results and Analysis

- The model successfully replicates the results from the referenced paper for small instances.  
- Larger instances confirm the **NP-hard nature** of the problem — solution time grows rapidly.  
- The SAA method provides stable expected-value approximations under varying scenario counts.  
- Performance metrics include:
  - Number of successful transplants  
  - Number of tests performed  
  - Runtime and solver gap  

## Key Insights

- The two-stage stochastic MILP effectively captures uncertainty in kidney exchange programs.  
- Scenario-based modeling (SAA) balances computational tractability and solution quality.  
- Scalability remains the main limitation; decomposition or heuristic approaches could improve efficiency.  

## Dependencies

- Python 3.x  
- Pyomo  
- Gurobi (or GLPK as fallback)  
- NumPy, Pandas, NetworkX, itertools  

Install with:
```
pip install pyomo gurobipy pandas numpy networkx
```
## Repository Structure
```
├── report/                                 # Model outputs, metrics, and plots  
├── Modeling & Sensetivity Analysis.ipynb    # Main implementation notebook  
└── README.md                                # Project description  
```

## Author
Ava Sedighi
Sharif University of Technology
Course: Healthcare Systems Engineering (Spring 2025)
