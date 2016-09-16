# lp-modeler
* A linear programming modeler written in Rust. This api helps to write LP model and use solver such as CBC, Gurobi, lp\_solve, ...*

## Usage
Dev in progress.


The first usable version will provide this SDL to make a LP Model :
```rust
use lp_modeler::problem::{LpObjective, LpProblem};
use lp_modeler::variables::{LpVariable, LpType, LpOperations};

let ref a = LpVariable::new("a", LpType::Integer);
let ref b = LpVariable::new("b", LpType::Integer);

let mut problem = LpProblem::new("One Problem", LpObjective::Maximize);

problem += 10 * a + 20 * b;
problem += (500 * a + 1200 * b).le(10000);
problem += (a).le(b);

problem.solve();
```

it will soon be possible to export this model 
into the [lp file format](https://www.gurobi.com/documentation/6.5/refman/lp_format.html "lp file format on Gurobi website"). 

```
problem.write_lp("problem.lp") 
```

will produce :

```
\ One Problem

Maximize
  10 a + 20 b

Subject To
  c1: 500 a + 1200 b <= -10000
  c2: a - b <= 0

Generals
  b a 

End
```

With this file, you can directly use it 
with a solver supporting lp file format :
* open source solvers :
    * lp_solve
    * glpk
    * cbc
* commercial solvers :
    * Gurobi
    * Cplex
    
## Todo
>* lp file format must have only constant literal on the right side of
   the expression. Working on doing this implicitly.
>* call directly the solver from this library

