# Callable Bermudan Exotic Option Valuation

A Bermudan callable structure is a structure consisting of two sets of cashflows, one paid and one received, which can be valued in the usual Monte Carlo setting, and a set of dates (notice dates) when the structure can optionally be cancelled (or called). When this happens all payments for future periods are stopped, and possibly a penalty payment is made

The choice of financial coordinates to be used is not an exact process and is driven largely by experience and experimentation. Good starting guesses for interest rate structures are a short rate, say 6M or whatever the funding rate is, and a co-terminal swap rate and also corresponds to our in-house experience. The informal justification is that the co-terminal swap rate captures the overall level of rates and the short Libor the local slope of rates curve. However, there are sometimes better choices, and given time one should experiment for every new trade type.

The choice of parametric forms for the boundary is another decision to be made mostly based on empirical results. The form can be quite general, restricted mostly by the number of parameters that can be optimised in a reasonable time, but this is not a major restriction. Simple forms are reported and observed by us to be working well for most practical problems (e.g. linear and quadratic, or even a simple optimised trigger value in some cases).

From the above discussion it is, we hope, clear that once the choices of a parametric form and financial coordinates for the exercise decision function are made, it remains to estimate the best values for the parameters. After that is done we can proceed with valuation as normal, simulating Monte Carlo paths and making a decision to exercise the option on each notice day,

The value of an option on a single simulation path is 0 if the call is not exercised, or if it is, a sum of: (i) a penalty payment fixed on the date when the call has been exercised (ii) cashflows opposite to all cancelled ones in the underlying structure The total value of the option is of course the average over all simulated paths, and the total value of a Bermudan callable structure is the sum of the value of the option and the values of its underlying legs.

The essential difficulty in estimating the best values for pi lays in the backwards induction nature of the optimal decision at every notice date - it depends on the hold value, which in turn depends on the optimal exercise decision on the next notice date. In other words: if hold values were known, we would know what the optimal decision is on each path, so we could in principle find values for pi that get our decision function as close to it as possible.

The solution to this is to simulate a number of paths, evaluating and recording values of financial coordinates and underlying structure for each path, and then optimise the parameters of the exercise decision function by backwards induction on the pre-calculated paths. The key insight is that once an entire set of paths has been simulated, estimates of hold and cancellation values are known and we can proceed with backwards induction.

Unfortunately, we cannot use these results directly as this introduces a perfect foresight bias. Intuitively: if we were to simply make the decisions on each simulated path by comparing pre-calculated hold and cancellation values we would get results better than we could reasonably hope to achieve in practice.

To remove this high bias we produce two sets of independent simulated paths. We use the first set for “training” of our parametric decision function, that is, we estimate the parameters using hold and cancellation values from this set. We then price the trade using the second set of paths and the “trained” decision function. To put it simply, we try to find a set of parameters pi that would have maximised the value of the option over this first set of paths, then we use these values to make decisions during valuation on another set of paths. The two sets of paths are independent (so we are rid of the high bias), but converge to the same mean (so our estimated parameters should be close enough to “real” values of the “best” parameters ).

There are, at present, two main approaches to estimating the parameters from the training set, using least squares regression (first described in [3]) and using maximisation of option value. The first approach uses least squares regression to fit the parametric representation of the hold value to pre-calculated results, using the fact that at each notice date we know exactly what the best possible decision would have been. The second approach uses numerical optimisation to search for the set of parameters that induces the highest value of the option, using pre-calculated hold values to compute the objective function at each step of numerical maximisation. The two approaches are equivalent, in that they use the same information, in the same order 13 and produce a set of parameters that maximise the option value, the only difference being the approach to optimisation.

Reference:

https://finpricing.com/product.html

