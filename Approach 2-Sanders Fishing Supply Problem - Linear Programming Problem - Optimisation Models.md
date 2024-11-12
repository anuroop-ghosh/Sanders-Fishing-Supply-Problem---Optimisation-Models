```python
#Importing necessary libraries
from pyomo.environ import *
import pandas as pd
```


```python
#Reading the problem from the excel data
file_name = 'Sanders Fishing Supply Problem - Linear Programming Problem - Optimisation Models.xlsx'
df = pd.read_excel(file_name, index_col=0)

df_np = df.to_numpy()
```


```python
#Importing each value into separate datasets
cost_type = df.loc[df.index[0],df.columns[5:]].keys()
month = df_np[0:3,4]
capacity_regular = df_np[0:3,0]
capacity_overtime = df_np[0:3,1]
demand = df_np[0:3,2]
cost_regular = df_np[0:3,5]
cost_overtime = df_np[0:3,6]
inventory_holding_cost = df_np[0:3,7]
```


```python
#Defining the model
model = ConcreteModel()

model.TL = Var(range(len(cost_type)*len(month)), domain = NonNegativeReals)
model.cons1 = Var(range(len(cost_type)), domain = NonNegativeReals)
model.cons2 = Var(range(len(cost_type)+1),domain = NonNegativeReals)
model.cons3 = Var(range(len(cost_type)),domain = NonNegativeReals)
model.cons4 = Var(range(1),domain = NonNegativeReals)
model.cons5 = Var(range(1),domain = NonNegativeReals)
model.cons6 = Var(range(1),domain = NonNegativeReals)
model.cons7 = Var(range(1),domain = NonNegativeReals)
model.cons8 = Var(range(1),domain = NonNegativeReals)
model.cons9 = Var(range(1),domain = NonNegativeReals)
```


```python
#Defining Objective Function Rule
def obj (model):
  func = 0
  i = 0
  for i in range(len(cost_type)):
    func += model.TL[i]*cost_regular[i]
    func += model.TL[i+3]*cost_overtime[i]
    func += model.TL[i+6]*inventory_holding_cost[i]
  return func
```


```python
model.cost = Objective(rule=obj, sense=minimize)
```


```python
model.cons1 = Constraint(expr = (model.TL[0]+model.TL[3]-model.TL[6]) ==demand[0])
model.cons2 = Constraint(expr = (model.TL[1]+model.TL[4]+model.TL[6]-model.TL[7]) ==demand[1])
model.cons3 = Constraint(expr = (model.TL[2]+model.TL[5]+model.TL[7]-model.TL[8]) ==demand[2])
model.cons4 = Constraint(expr = model.TL[0] <= capacity_regular[0])
model.cons5 = Constraint(expr = model.TL[3] <= capacity_overtime[0])
model.cons6 = Constraint(expr = model.TL[1] <= capacity_regular[1])
model.cons7 = Constraint(expr = model.TL[4] <= capacity_overtime[1])
model.cons8 = Constraint(expr = model.TL[2] <= capacity_regular[2])
model.cons9 = Constraint(expr = model.TL[5] <= capacity_overtime[2])
```

    WARNING: Implicitly replacing the Component attribute cons1 (type=<class
    'pyomo.core.base.var.IndexedVar'>) on block unknown with a new Component
    (type=<class 'pyomo.core.base.constraint.AbstractScalarConstraint'>). This is
    usually indicative of a modelling error. To avoid this warning, use
    block.del_component() and block.add_component().
    WARNING: Implicitly replacing the Component attribute cons2 (type=<class
    'pyomo.core.base.var.IndexedVar'>) on block unknown with a new Component
    (type=<class 'pyomo.core.base.constraint.AbstractScalarConstraint'>). This is
    usually indicative of a modelling error. To avoid this warning, use
    block.del_component() and block.add_component().
    WARNING: Implicitly replacing the Component attribute cons3 (type=<class
    'pyomo.core.base.var.IndexedVar'>) on block unknown with a new Component
    (type=<class 'pyomo.core.base.constraint.AbstractScalarConstraint'>). This is
    usually indicative of a modelling error. To avoid this warning, use
    block.del_component() and block.add_component().
    WARNING: Implicitly replacing the Component attribute cons4 (type=<class
    'pyomo.core.base.var.IndexedVar'>) on block unknown with a new Component
    (type=<class 'pyomo.core.base.constraint.AbstractScalarConstraint'>). This is
    usually indicative of a modelling error. To avoid this warning, use
    block.del_component() and block.add_component().
    WARNING: Implicitly replacing the Component attribute cons5 (type=<class
    'pyomo.core.base.var.IndexedVar'>) on block unknown with a new Component
    (type=<class 'pyomo.core.base.constraint.AbstractScalarConstraint'>). This is
    usually indicative of a modelling error. To avoid this warning, use
    block.del_component() and block.add_component().
    WARNING: Implicitly replacing the Component attribute cons6 (type=<class
    'pyomo.core.base.var.IndexedVar'>) on block unknown with a new Component
    (type=<class 'pyomo.core.base.constraint.AbstractScalarConstraint'>). This is
    usually indicative of a modelling error. To avoid this warning, use
    block.del_component() and block.add_component().
    WARNING: Implicitly replacing the Component attribute cons7 (type=<class
    'pyomo.core.base.var.IndexedVar'>) on block unknown with a new Component
    (type=<class 'pyomo.core.base.constraint.AbstractScalarConstraint'>). This is
    usually indicative of a modelling error. To avoid this warning, use
    block.del_component() and block.add_component().
    WARNING: Implicitly replacing the Component attribute cons8 (type=<class
    'pyomo.core.base.var.IndexedVar'>) on block unknown with a new Component
    (type=<class 'pyomo.core.base.constraint.AbstractScalarConstraint'>). This is
    usually indicative of a modelling error. To avoid this warning, use
    block.del_component() and block.add_component().
    WARNING: Implicitly replacing the Component attribute cons9 (type=<class
    'pyomo.core.base.var.IndexedVar'>) on block unknown with a new Component
    (type=<class 'pyomo.core.base.constraint.AbstractScalarConstraint'>). This is
    usually indicative of a modelling error. To avoid this warning, use
    block.del_component() and block.add_component().



```python
#Checking the model
model.pprint()
```

    1 Var Declarations
        TL : Size=9, Index={0, 1, 2, 3, 4, 5, 6, 7, 8}
            Key : Lower : Value : Upper : Fixed : Stale : Domain
              0 :     0 :  None :  None : False :  True : NonNegativeReals
              1 :     0 :  None :  None : False :  True : NonNegativeReals
              2 :     0 :  None :  None : False :  True : NonNegativeReals
              3 :     0 :  None :  None : False :  True : NonNegativeReals
              4 :     0 :  None :  None : False :  True : NonNegativeReals
              5 :     0 :  None :  None : False :  True : NonNegativeReals
              6 :     0 :  None :  None : False :  True : NonNegativeReals
              7 :     0 :  None :  None : False :  True : NonNegativeReals
              8 :     0 :  None :  None : False :  True : NonNegativeReals
    
    1 Objective Declarations
        cost : Size=1, Index=None, Active=True
            Key  : Active : Sense    : Expression
            None :   True : minimize : 45.0*TL[0] + 55.0*TL[3] + 12.0*TL[6] + 45.0*TL[1] + 70.0*TL[4] + 12.0*TL[7] + 75.0*TL[2] + 100.0*TL[5] + 12.0*TL[8]
    
    9 Constraint Declarations
        cons1 : Size=1, Index=None, Active=True
            Key  : Lower : Body                  : Upper : Active
            None : 110.0 : TL[0] + TL[3] - TL[6] : 110.0 :   True
        cons2 : Size=1, Index=None, Active=True
            Key  : Lower : Body                          : Upper : Active
            None : 155.0 : TL[1] + TL[4] + TL[6] - TL[7] : 155.0 :   True
        cons3 : Size=1, Index=None, Active=True
            Key  : Lower : Body                          : Upper : Active
            None : 160.0 : TL[2] + TL[5] + TL[7] - TL[8] : 160.0 :   True
        cons4 : Size=1, Index=None, Active=True
            Key  : Lower : Body  : Upper : Active
            None :  -Inf : TL[0] :  85.0 :   True
        cons5 : Size=1, Index=None, Active=True
            Key  : Lower : Body  : Upper : Active
            None :  -Inf : TL[3] : 115.0 :   True
        cons6 : Size=1, Index=None, Active=True
            Key  : Lower : Body  : Upper : Active
            None :  -Inf : TL[1] : 130.0 :   True
        cons7 : Size=1, Index=None, Active=True
            Key  : Lower : Body  : Upper : Active
            None :  -Inf : TL[4] : 145.0 :   True
        cons8 : Size=1, Index=None, Active=True
            Key  : Lower : Body  : Upper : Active
            None :  -Inf : TL[2] : 130.0 :   True
        cons9 : Size=1, Index=None, Active=True
            Key  : Lower : Body  : Upper : Active
            None :  -Inf : TL[5] :  25.0 :   True
    
    11 Declarations: TL cost cons1 cons2 cons3 cons4 cons5 cons6 cons7 cons8 cons9



```python
#Solving the model
solver = SolverFactory('glpk')
results = solver.solve(model)
```


```python
#Printing the result
print('Cost = ', model.cost())
```

    Cost =  24845.0



```python

```
