# LinQuadOptInterface.jl

LinQuadOptInterface (LQOI) is designed to make it easier for low-level wrapper designed to bridge low-level Integer Linear and Quadratic solvers to implement the [MathOptInterface](https://github.com/JuliaOpt/MathOptInterface.jl) (MOI). It provides access to many MOI functionalities mainly related to problem modifications:

1. add constraints/variables 1-by-1 and in batches
2. remove constraints/variables
3. change constraint coefficients
4. change constraint type
5. change constraint rhs
6. change variable bounds and type

The use of LQOI for MOI implementations is entirely optional. Using LQOI introduces an extra abstraction layer between a solver and MOI. Its recommended to carefully analyse if the solver's low-level API is close to what LQOI expects, otherwise a direct implementation of MOI might be a better option.

## LinQuadOptInterface Optimizer

In LinQuadOptInterface.jl the MOI `AbstractOptimizer` is specialized to `LinQuadOptimizer`. In this implementation all the above mentioned modifications are carried out by the low-level solver's own functionalities and hence the `LinQuadOptimizer` can be used without a `CachingOptimizer`. The latter will keep all problem data in the julia level and typically push to a low-level solver the complete problem at once.

Here the data in the julia level is minimal, basically only references to constraints and variables.

## Current uses

This package is currently used to implement [MathOptInterfaceXpress.jl](https://github.com/JuliaOpt/MathOptInterfaceXpress.jl), [MathOptInterfaceCPLEX.jl](https://github.com/JuliaOpt/MathOptInterfaceCPLEX.jl), [MathOptInterfaceGurobi.jl](https://github.com/JuliaOpt/MathOptInterfaceGurobi.jl) and [MathOptInterfaceGLPK.jl](https://github.com/JuliaOpt/MathOptInterfaceGLPK.jl). The last one being only a integer linear solver.

All these solvers have low-level APIs which supports most of these modifications. Hence, data storage is simplified and duplications are avoided.

## Other possible uses

This package is only recommended if a solver low-level API which supports most of these modifications.

If a solver low-level API does not support most of these modifications, then following the example of [MathOptInterfaceSCS.jl](https://github.com/JuliaOpt/MathOptInterfaceSCS.jl) and [MathOptInterfaceECOS.jl](https://github.com/JuliaOpt/MathOptInterfaceECOS.jl) might be a better idea.
