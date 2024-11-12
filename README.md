**README: Sanders Fishing Supply Problem - Linear Programming**

This Python code implements a linear optimization model to solve the following problem.

**Problem**

Sanders Fishing Supply of Naples, Florida, manufactures a variety of fishing equipment, which it sells throughout the United States. For the next three months, Sanders estimates demand for a particular product at 110, 155, and 160 units, respectively. Sanders can supply this demand by producing on regular time or overtime. Because of other commitments and anticipated cost increases in month 3, the production capacities in units and the production costs per unit are as follows:
| Month | Production Type | Capacity | Cost/unit |
|---|---|---|---|
| Month 1 | Regular | 85 | $45.00 |
| Month 1 | Overtime | 115 | $55.00 |
| Month 2 | Regular | 130 | $45.00 |
| Month 2 | Overtime | 145 | $70.00 |
| Month 3 | Regular | 130 | $75.00 |
| Month 3 | Overtime | 25 | $100.00 |

Inventory may be carried from one month to the next, but the holding cost is $12 per unit per month.
Objective:
Determine the minimal cost of meeting demands in the next three months.

**Problem Description:**

This Python code, built using the Pyomo library, solves a linear programming problem to optimize the production schedule for Sanders Fishing Supply. The goal is to minimize the total cost of production, including regular and overtime costs, while meeting demand constraints and considering inventory holding costs.

**Code Structure:**

1. **Import Libraries:**
   - `pyomo.environ`: Provides the modeling framework.
   - `pandas`: Used for data manipulation and reading from Excel.
   - `numpy`: For numerical operations.

2. **Data Import:**
   - Reads the problem data from an Excel file.
   - Extracts relevant information:
     - Month, regular capacity, overtime capacity, demand, regular cost, overtime cost, and inventory holding cost for each month.

3. **Model Definition:**
   - Creates a ConcreteModel instance.
   - Defines decision variables:
     - `x`: Regular production quantity for each month.
     - `y`: Overtime production quantity for each month.
     - `z`: Inventory level for each month.

4. **Objective Function:**
   - Minimizes the total cost, considering regular, overtime, and inventory holding costs.

5. **Constraints:**
   - **Regular Capacity:** Ensures regular production doesn't exceed regular capacity.
   - **Overtime Capacity:** Ensures overtime production doesn't exceed overtime capacity.
   - **Demand:** Ensures demand is met for each month, considering inventory.

6. **Solver and Solution:**
   - Uses the GLPK solver to solve the model.
   - Prints the optimal solution:
     - Optimal regular and overtime production for each month.
     - Optimal inventory level for each month.
     - Minimum total cost.

**Customization:**

- **Data:** You can modify the Excel file to change the problem parameters and re-run the code.
- **Solver:** You can experiment with different solvers available in Pyomo.

**Additional Considerations:**

- **Approach 1:** The provided code is a more generalized approach that can be adapted to various production planning problems.
- **Approach 2:** While not implemented here, a more specific approach could be tailored to the exact problem constraints and data.
- **Sensitivity Analysis:** Consider performing sensitivity analysis to understand how changes in parameters affect the optimal solution.

**Note:** Ensure that you have Pyomo and GLPK installed in your Python environment to run this code.
