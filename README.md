# piratefly-mcp

**Is this flight price good right now?** An [MCP](https://modelcontextprotocol.io) server that
answers *buy or wait* for a route, from 90 days of observed fares — not from today's snapshot.

Powered by [Piratefly](https://piratefly.com/?src=github-mcp), which records flight prices every day and keeps the
history. No API key, no account, no ticket selling.

## Install

The server is live over MCP Streamable HTTP — no install, no API key:

```bash
claude mcp add --transport http piratefly https://piratefly.com/mcp
```

Any MCP client with remote HTTP support: point it at `https://piratefly.com/mcp`.

The stdio bridge in `bin/` exists for clients that only speak stdio, and runs the same endpoint:

```bash
node bin/piratefly-mcp.mjs
```

Prefer the remote endpoint: it needs nothing installed.

## Tools

| Tool | Answers |
| --- | --- |
| `flight_price_verdict` | Buy now or wait? Verdict, percentile of today's cheapest fare against its own history, median, all-time low, sample size. |
| `cheapest_months` | Which departure months are cheapest for this route, by observed median price. |
| `when_to_book` | How many days before departure this route has historically been cheapest. |

Each takes `origin` and `destination` — city name or IATA code (`"Milan"`, `"MXP"`).

Ask your assistant: *"Is €480 a good price for Milan to Tokyo in March?"*

Want to see the data first? Every watched route has a public price-history page, e.g.
[Paris → Milan](https://piratefly.com/flights/paris-to-milan?src=github-mcp) or
[London → Barcelona](https://piratefly.com/flights/london-to-barcelona?src=github-mcp). The full
daily dataset is open: [flight-fare-history](https://github.com/DeusAcc/flight-fare-history).

## What it is not

Not a flight search engine, and it does not sell anything. It judges a price against the history of
that route, and answers only for routes Piratefly actually watches.

## How it works

The package is a ~50-line, zero-dependency stdio bridge: it forwards JSON-RPC messages to the
hosted server at `https://piratefly.com/mcp`. Override with `PIRATEFLY_MCP_URL`.

MIT licensed. Data: Piratefly flight price history — https://piratefly.com/?src=github-mcp
