# Verus by Markst — Claude plugin

Price, stress and explain private-credit loans from Claude. The plugin connects Claude Code and Cowork to the Verus MCP server at `https://mcp.markst.com/` and adds a skill that tells Claude how to use the valuation tools well.

Verus is the deterministic valuation engine behind [Markst](https://markst.com), an independent private-credit valuation firm. Same inputs always produce the same price. There is no machine-learning pricing and no randomness.

## What you need

A Verus account. Sign in at [beta.markst.com](https://beta.markst.com). The first time a tool is called, Claude opens the Verus consent screen in your browser and you choose read-only or read-and-write access. Verus issues Claude a token; your password never reaches Claude.

## Install

```bash
claude plugin marketplace add markstltd/verus-claude-plugin
claude plugin install verus@markst
```

Or, without the plugin, add the server directly in any MCP client:

```json
{ "mcpServers": { "verus": { "type": "http", "url": "https://mcp.markst.com/" } } }
```

## Tools

| Tool | Access | Purpose |
|---|---|---|
| `list_deals` | read | Deals visible to you |
| `get_deal` | read | Facility terms and version pointers for one deal |
| `list_scenarios` | read | Predefined macro and sector stress scenarios |
| `find_comparables` | read | Comparable deals with implied spreads |
| `portfolio_view` | read | Fund positions, NAV, concentrations |
| `value_deal` | write | DCF price with low / mid / high range and credit metrics |
| `multi_factor_price` | write | Factor-based rating, score and spread for direct lending, mezzanine, CRE and distressed |
| `sensitivity_ladder` | write | Bump-and-reprice ladder on spread, hazard, recovery or rates |
| `run_scenario` | write | Stressed valuations across the portfolio |
| `create_deal_from_text` | write | Register a deal from a pasted term sheet or memo |

Write tools persist a valuation run so every number is auditable. Nothing deletes or overwrites.

## Privacy and support

- Privacy notice: https://markst.com/legal/privacy
- Terms: https://markst.com/legal/terms
- Support: info@markst.com

Data you send through the tools is processed under the Verus terms for your account. The plugin itself stores nothing.

## Licence

MIT for the plugin files in this repository. Verus and the Markst service are licensed separately under their own terms.
