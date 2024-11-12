```python
#Importing necessary libraries
from pyomo.environ import *
import pandas as pd
import numpy as np
```


```python
#Reading the problem from the excel data
file_name = 'Sanders Fishing Supply Problem - Linear Programming Problem - Optimisation Models.xlsx'
df = pd.read_excel(file_name, index_col=0)
df_np = df.to_numpy()
```


```python
#Importing each value into separate datasets
cost_type = df.loc[df.index[0],df.columns[5:]].keys() # Cost types from the DataFrame
month = df_np[0:3,4] # Month data
capacity_regular = df_np[0:3,0] # Regular capacity for each month
capacity_overtime = df_np[0:3,1] # Overtime capacity for each month
demand = df_np[0:3,2] # Demand for each month
cost_regular = df_np[0:3,5] # Regular costs
cost_overtime = df_np[0:3,6] # Overtime costs
inventory_holding_cost = df_np[0:3,7] # Inventory holding costs
no_of_months = len(month) #Total number of months
```


```python
#Defining the model
model = ConcreteModel()
model.x = Var(range(no_of_months), domain=NonNegativeReals) # Regular
model.y = Var(range(no_of_months), domain=NonNegativeReals) # Overtime
model.z = Var(range(no_of_months), domain=NonNegativeReals) # Inventory
```


```python
# Objective function: minimize the total cost, including regular costs, overtime costs, and inventory holding costs
def objective_rule(model):
    return sum(cost_regular[i]*model.x[i] + cost_overtime[i]*model.y[i] + inventory_holding_cost[i]*model.z[i] for i in range(no_of_months))
model.cost = Objective(rule=objective_rule, sense=minimize)
```


```python
# Regular capacity constraint: ensure regular production does not exceed available capacity each month
def capacity_rule_regular(model, i):
    return model.x[i] <= capacity_regular[i]
model.capacity_regular = Constraint(range(no_of_months), rule=capacity_rule_regular)
```


```python
# Overtime capacity constraint: ensure overtime production does not exceed available capacity each month
def capacity_rule_overtime(model, i):
    return model.y[i] <= capacity_overtime[i]
model.capacity_overtime = Constraint(range(no_of_months), rule=capacity_rule_overtime)
```


```python
# Demand constraint: ensure that production meets demand for each month, accounting for inventory levels
def demand_rule(model, i):
  if i == 0:
    return model.x[i] + model.y[i] - model.z[i] == demand[i]
  else:
    return model.x[i] + model.y[i] + model.z[i-1] - model.z[i] == demand[i]
model.demand = Constraint(range(no_of_months), rule=demand_rule)
```


```python
#Solving the model
solver = SolverFactory('glpk')
results = solver.solve(model)
```


```python
#Printing the results
if results.solver.termination_condition == TerminationCondition.optimal:
    opt_sol7 = np.zeros(len(month))
    for i in range (len(month)):
      opt_sol7[i] = model.x[i]()
      print('Optimal regular production in month', i+1, 'is ', model.x[i]())
      print('Optimal overtime production in month', i+1, 'is ', model.y[i]())
      print('Optimal inventory in month', i+1, 'is ', model.z[i]())
    print('Minimum Cost is', model.cost())

else:
    print("Solve failed.") 
```

    Optimal regular production in month 1 is  85.0
    Optimal overtime production in month 1 is  80.0
    Optimal inventory in month 1 is  55.0
    Optimal regular production in month 2 is  130.0
    Optimal overtime production in month 2 is  0.0
    Optimal inventory in month 2 is  30.0
    Optimal regular production in month 3 is  130.0
    Optimal overtime production in month 3 is  0.0
    Optimal inventory in month 3 is  0.0
    Minimum Cost is 24845.0



```python

```
