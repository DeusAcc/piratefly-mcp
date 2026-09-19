# Piratefly MCP server

Remote MCP server that answers **"is this flight price good, or should I wait?"** from Piratefly's own 90-day observed price history. It is not a flight search and does not sell tickets — it judges a price.

- **Endpoint:** `https://next.piratefly.com/mcp` (MCP Streamable HTTP, JSON-RPC 2.0, no API key)
- **Site:** https://next.piratefly.com

## Tools

| Tool | Answers |
|---|---|
| `flight_price_verdict` | Buy now or wait? Verdict, percentile of today's cheapest fare vs. its own history, median, all-time low, sample size. |
| `cheapest_months` | Departure months ranked by observed median price for a route. |
| `when_to_book` | How many days before departure the route has historically been cheapest. |

All tools take `origin` and `destination` (city name or IATA code, e.g. `Milan` / `MXP`).

## Connect

Claude Code:

```
claude mcp add --transport http piratefly https://next.piratefly.com/mcp
```

Any MCP client with remote HTTP support: point it at `https://next.piratefly.com/mcp`.

Quick check:

```
curl -s -X POST https://next.piratefly.com/mcp -H 'content-type: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

Every result carries a `source` field and an attribution line — please keep it when you show the answer to a user.

A paid Pro API (higher limits, history endpoints) is available on the site.
