# Crowded Trades: How Competition Erodes Edge in Financial Markets

What happens to your trading edge when everyone else discovers it too? This project
answers that question with two classic, genuinely game-theoretic results from market
microstructure — solved in closed form and checked against Monte Carlo simulation.

## The two questions

1. **What if several traders all have the *same* private information?**
   (Kyle, 1985 → Holden & Subrahmanyam, 1992)

   One monopolist insider with secret information about an asset's value can extract
   a predictable profit trading against random noise traders. But what happens when
   `N` traders all have the *same* edge and compete simultaneously? This project
   derives the Nash equilibrium and shows that competition erodes profits to zero as
   `N → ∞` — even though, surprisingly, each *individual* trader actually scales back
   their own aggressiveness as competition increases.

2. **What happens when more dealers compete to provide liquidity?**

   Market makers choosing how much depth to offer, modeled as a Cournot oligopoly.
   More competitors → tighter bid-ask spreads → spreads converge to marginal cost,
   exactly the textbook industrial-organization result, here applied to trading
   instead of widgets.

## The result

| | N = 1 | N = 5 | N = 50 |
|---|---|---|---|
| Individual trader's intensity β(N) | 1.50 | 0.67 | 0.21 |
| Total profit extracted | 3.00 | 2.24 | 0.83 |
| Equilibrium spread (a=1, b=0.01, c=0.10) | 0.55 | 0.25 | 0.12 |

Two sides of the same coin: **adding competitors doesn't eliminate the underlying
edge or friction — it just forces it to be priced away.**

![Results](oligopoly_results.png)

## Why this is "crowded trades," not just theory

This is the formal version of a real and common situation in markets: a signal that
works great when only you (or a few others) trade on it decays fast once everyone
piles in. The model shows the decay isn't smooth or proportional — individual traders
rationally pull back even as the *collective* damage to the opportunity accelerates.
The same logic governs why bid-ask spreads compress as more market makers enter a
name. Useful intuition for sizing positions, judging how "alive" a signal still is,
and reasoning about whether a liquidity-provision business is still worth running.

## Running it

```bash
pip install numpy matplotlib
python oligopoly.py
```

No other dependencies, no data files, no network access required. The script prints:

1. The single-insider (monopoly) baseline
2. The N-insider Nash equilibrium across N = 1 → 100
3. An independent verification of the closed-form solution via best-response
   (fictitious-play) iteration
4. A 100,000-trial Monte Carlo check that the equilibrium is internally consistent
5. The Cournot market-maker spread across N = 1 → 200
6. A heterogeneous-cost example (what happens when one dealer has a cost advantage)

...and saves `oligopoly_results.png` with four summary charts.

## The math, briefly

**N-insider Nash equilibrium**, with `N` symmetric traders sharing signal
`v ~ N(0, σᵥ²)` against noise flow `u ~ N(0, σᵤ²)`:

```
β(N) = σᵤ / (σᵥ√N)            individual trading intensity
λ(N) = σᵥ√N / (σᵤ(N+1))       price impact ("Kyle's lambda")
π(N) = σᵤσᵥ / (√N(N+1))       expected profit per trader
```

At `N=1` this is exactly Kyle (1985). As `N → ∞`, total profit `N·π(N) → 0`.

**Cournot liquidity provision**: `N` market makers choose depth `qᵢ` to maximize
`(s(Q) − cᵢ)·qᵢ` against inverse demand `s(Q) = a − bQ`. Symmetric-cost equilibrium
spread: `s* = (a + Nc)/(N+1) → c` as `N → ∞`.

Full derivations are in the function docstrings.

## References

- Kyle, A. S. (1985). *Continuous Auctions and Insider Trading*. Econometrica.
- Holden, C. W., & Subrahmanyam, A. (1992). *Long-Lived Private Information and
  Imperfect Competition*. Journal of Finance.
- Standard N-firm Cournot oligopoly (any intermediate microeconomics / industrial
  organization text), applied here to liquidity provision.

## License

MIT
