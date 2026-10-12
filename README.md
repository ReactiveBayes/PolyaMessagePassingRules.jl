# PolyaMessagePassingRules

[![CI](https://github.com/ReactiveBayes/PolyaMessagePassingRules.jl/actions/workflows/CI.yml/badge.svg?branch=main)](https://github.com/ReactiveBayes/PolyaMessagePassingRules.jl/actions/workflows/CI.yml)
[![Docs: stable](https://img.shields.io/badge/docs-stable-blue.svg)](https://reactivebayes.github.io/PolyaMessagePassingRules.jl/stable/)
[![Docs: dev](https://img.shields.io/badge/docs-dev-blue.svg)](https://reactivebayes.github.io/PolyaMessagePassingRules.jl/dev/)
[![Coverage](https://codecov.io/gh/ReactiveBayes/PolyaMessagePassingRules.jl/graph/badge.svg)](https://codecov.io/gh/ReactiveBayes/PolyaMessagePassingRules.jl)
[![Code style: Runic](https://img.shields.io/badge/code_style-%E1%9A%B1%E1%9A%A2%E1%9A%BE%E1%9B%81%E1%9A%B2-black)](https://github.com/fredrikekre/Runic.jl)

**This package is licensed under GPL-3**, because its dependency PolyaGammaHybridSamplers is.
The engine and the other rule packages are MIT and do not depend on it.

The Pólya-Gamma augmented nodes for count data through the logistic, whose messages towards the
weights are normal:

- `BinomialPolya`, `y ~ Binomial(n, σ(xᵀβ))`: binomial and logistic regression, under
  `BinomialPolyaApproximation(; samples = nothing)`;
- `MultinomialPolya`, `x ~ Multinomial(N, p(ψ))` by logistic stick-breaking: multinomial
  regression, under `MultinomialPolyaApproximation(; points = 21)`.

Both algorithms are the nodes' defaults. The rules towards the weights read the message on the
weights' own edge, which a model must initialise.

```julia
using PolyaMessagePassingRules, MessagePassingRulesBase, BayesBase, ExponentialFamily

result = @call_message_update_rule(
    node = BinomialPolya, target = :β,
    q = (y = PointMass(3), x = PointMass([0.1, 0.2]), n = PointMass(5)),
    m = (β = MvNormalWeightedMeanPrecision(zeros(2), [1.0 0.0; 0.0 1.0]),),
)
getresult(result)   # MvNormalWeightedMeanPrecision, ξ = (3 - 5/2) x, Λ = (5/4) x xᵀ
```

```julia
import Pkg; Pkg.add("PolyaMessagePassingRules")
```

- Documentation: <https://reactivebayes.github.io/PolyaMessagePassingRules.jl/stable/>.
  `make docs` builds it locally, into `docs/build`.
- Tests: `make test` runs the suite as CI does, ending with the rule-coverage gate: every rule
  must be selected by some test. `make test test_args="name:rules"` selects items by tag, by
  name or by file; `make help` lists the other targets.
- Depends on MessagePassingRulesBase, MessagePassingRulesApproximations (Gauss–Hermite
  cubature), BayesBase, ExponentialFamily, Distributions, LogExpFunctions, SpecialFunctions and
  PolyaGammaHybridSamplers.
- Licence: **GPL-3**.
