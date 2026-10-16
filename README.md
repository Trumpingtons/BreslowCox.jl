# BreslowCox.jl

The Cox model with Breslow's ties, and its predicted survival.

The Cox proportional hazards model fitted on Breslow's partial likelihood, as SAS PHGLM did, with Wald and likelihood ratio tests, and Kalbfleisch and Prentice's predicted survival, tested on the Mayo model of primary biliary cirrhosis.

Article: Dickson, E. R., Grambsch, P. M., Fleming, T. R., Fisher, L. D. and Langworthy, A. (1989). Prognosis in primary biliary cirrhosis: model for decision making. Hepatology 10(1), 1-7. [doi:10.1002/hep.1840100102](https://doi.org/10.1002/hep.1840100102)

## Usage

```julia
using BreslowCox, CSV, Tables
pbc = Tables.columntable(CSV.File(download("https://raw.githubusercontent.com/Trumpingtons/PollisDatasets.jl/main/data/pbc.csv")))
X = Float64.(hcat(log.(pbc.bili), log.(pbc.albumin), pbc.age, pbc.edema))
m = cox(X, Float64.(pbc.time), pbc.status .== 2)  # Breslow's ties
m.β, m.se, m.p
S = survival(m)  # S(t, R): the predicted survival at t days of a patient with risk score R
S(5 * 365.25, X[1, :]' * m.β)
```

## Vignette

[The Cox model with Breslow's ties, and its predicted survival](vignette/BreslowCox.pdf)

## License

MIT (see [LICENSE](LICENSE)).

## Tests

The tests compute the published tables below from their data and check every printed value, rounded to the digits printed:

- Likelihood ratio chi-square of the initial model, p. 4 (`test/data/targets/likelihoodratio.csv`)
- Table 2, final model, p. 4 (two prothrombin times estimated more simply than the paper's) (`test/data/targets/table2final.csv`)
- Table 2, initial model, p. 4 (`test/data/targets/table2initial.csv`)
- Table 4, p. 6 (`test/data/targets/table4.csv`)

The data, downloaded from its links when the tests run or kept in `test/data/inputs`:

- `pbc`: <https://raw.githubusercontent.com/Trumpingtons/PollisDatasets.jl/main/data/pbc.csv>, PollisDatasets: the 418 Mayo patients as Fleming and Harrington (1991) published them

Run them with `using Pkg; Pkg.test("BreslowCox")`.

Made with [Pollis](https://pollis.org).
