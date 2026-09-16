---
name: verus-valuation
description: Price, stress and explain private-credit loans with the Verus MCP tools. Use when the user asks about deals, marks, prices, spreads, scenarios, sensitivities, comparables or portfolio composition, or pastes a term sheet to register as a deal.
---

# Working with Verus

Verus is a deterministic valuation engine for direct lending. Same inputs always produce the same price. Every number you report must come from a tool result; never estimate a price, spread or NAV yourself.

## Which tool

| User asks | Call |
|---|---|
| What deals do I have / pipeline / portfolio | `list_deals`, then `portfolio_view(fund_id)` for NAV and concentrations |
| Tell me about a deal, its terms | `get_deal(deal_id)` |
| Price / mark / value a deal | `value_deal(deal_id)` |
| Score a deal from factors, no cashflow model yet | `multi_factor_price(strategy, inputs)` |
| If spreads widen 100 bps, what is the mark | `sensitivity_ladder(deal_id, parameter, shocks)` after at least one `value_deal` |
| What if recession / sector downturn | `list_scenarios`, then `run_scenario(scenario_ids)` |
| Comps, is this priced in line with the market | `find_comparables(deal_id)` |
| Here is a term sheet, set it up | `create_deal_from_text(prompt)` |

Chain freely in one turn: `list_deals` → `value_deal` → `sensitivity_ladder`.

## Reading a valuation

- Prices are per 100 of **current outstanding** notional. Position value = price / 100 × `principal_factor` × original commitment.
- Check `range_basis` before describing low / mid / high. Under `scenario_blend` the low is the recovery-scenario price and the high is the base-scenario price. They are scenario bounds, not percentiles. Under `spread_percentile` they are the 5th, 50th and 95th percentiles of the spread distribution.
- Spreads are in basis points everywhere.
- Report the range and the central case, name the two or three drivers that matter, and state what would move the mark.

## Read versus write

`list_deals`, `get_deal`, `list_scenarios`, `find_comparables` and `portfolio_view` only read. `value_deal`, `multi_factor_price`, `sensitivity_ladder` and `run_scenario` persist a valuation run so it is auditable. `create_deal_from_text` creates a deal record and may search the web to fill gaps. Confirm with the user before creating a deal from text they did not explicitly ask you to register.

## Errors

A tool error naming a missing scope means the connection was authorised read-only. Ask the user to reconnect Verus and grant write access. A 404 on a deal id means it is not in a fund the user belongs to.
