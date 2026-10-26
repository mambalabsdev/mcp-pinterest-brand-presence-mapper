# Pinterest Brand Presence Mapper MCP Server

[![LightNow capabilities](https://lightnow.ai/badge/com.mambabuilt/mcp-pinterest-brand-presence-mapper)](https://lightnow.ai/servers/com.mambabuilt/mcp-pinterest-brand-presence-mapper) [![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-pinterest-brand-presence-mapper/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-pinterest-brand-presence-mapper) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-pinterest-brand-presence-mapper)](https://www.npmjs.com/package/@mambalabsdev/mcp-pinterest-brand-presence-mapper) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-pinterest-brand-presence-mapper)](https://www.npmjs.com/package/@mambalabsdev/mcp-pinterest-brand-presence-mapper) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-pinterest-brand-presence-mapper)](https://github.com/mambalabsdev/mcp-pinterest-brand-presence-mapper/blob/main/LICENSE)

<a href="https://mcprush.com/to/pinterest-brand-presence-mapper-mcp?ref=badge"><img width="380" height="200" src="https://mcprush.com/badge/pinterest-brand-presence-mapper-mcp/card.svg" alt="Pinterest Brand Presence Mapper MCP server" /></a>

MCP server for the Mamba Labs **Pinterest Brand Presence Mapper** actor on Apify.

Resolve a company domain to its Pinterest business account with exact follower, pin and board counts.

## What it does

Resolve a company domain to its Pinterest business account and return EXACT follower, following, pin and board counts, plus the claimed website, verified merchant status and last pin date, as one flat Clay ready row. Pinterest serves real integers, so these counts can be summed across a list. Board count is a better activity signal than followers for a consumer brand, because boards are curation effort. Pinterest handles are rarely the domain stem, so a guessed account that fails the identity check is reported as identity_mismatch with no counts rather than returning a stranger's numbers. Read only; requires an APIFY_TOKEN and consumes Apify credits per call.

## Quick start

Add this to your MCP client configuration:

```json
{
  "mcpServers": {
    "mamba-pinterest-brand-presence-mapper": {
      "command": "npx",
      "args": ["-y", "@mambalabsdev/mcp-pinterest-brand-presence-mapper"],
      "env": { "APIFY_TOKEN": "your-apify-token" }
    }
  }
}
```

## Prerequisites

- Node.js 18 or newer
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations)

The actor is pay per event and consumes Apify credits per call. Pricing is on the
[actor page](https://apify.com/mambalabs/pinterest-brand-presence-mapper).

## Example prompts

- "How many Pinterest followers and boards does brooklinen.com have?"
- "Is chewy.com a verified merchant on Pinterest?"
- "Compare Pinterest board counts across patagonia.com and article.com."

## Tool and inputs

Tool: `map_pinterest_brand_presence`

| Input | Type | Meaning |
|---|---|---|
| `company_domain` | string | Bare company domain, for example shopify.com. Supply this or a handle. With a domain the actor runs full discovery; with a handle it skips straight to the fetch. |
| `company_name` | string | Optional. Improves search accuracy and is what the identity gate checks a discovered profile against, so supplying it reduces wrong matches. |
| `handle` | string | Optional. The Pinterest username from pinterest.com/<handle>. Supplying it skips discovery and, more importantly, skips the identity risk: Pinterest handles are rarely the domain stem. |
| `includeFollowerCounts` | boolean | Default true: the profile page is fetched and the follower counts are extracted. Set false to resolve the profile URL only, which is cheaper and needs no proxy. |
| `skipCache` | boolean | Default false: a successful lookup is cached for seven days and reused. Set true to force a fresh fetch. |
| `max_charge_usd` | number | Optional spending cap for this call, in USD. The actor stops before any charge that would take the run past this amount and keeps every row already charged. |

## Reading the output

Every row carries a per platform `_status` field, and it is the field to read
first. The vocabulary is the same across the whole Mamba Labs social family:

| Status | Meaning |
|---|---|
| `ok` | fetched and parsed, the value is there |
| `not_found` | we looked and there is no such profile |
| `not_extractable` | the profile exists and the value is not on the wire to us |
| `blocked` | the platform refused us, worth retrying later |
| `identity_mismatch` | we found a real profile and it belongs to someone else |
| `skipped` | you did not ask for this platform |

**`false` and `null` are never interchangeable.** `false` means we looked and the
answer is no. `null` means we could not look. If you filter for companies with no
presence, filter on `false`, because `null` rows are unknown rather than absent.

## Pricing

You pay per event on Apify, billed to your own Apify account. Prices from the live pricing record, read 2026-10-16:

| Event | Price per event (USD) | Fires when |
| --- | --- | --- |
| `apify-actor-start` | $0.00005 | Charged when the Actor starts running. Number of events charged depends on Actor memory (one event per GB, minimum one event). |
| `company-checked` | $0.0035 on the Free plan, down to $0.00245 on higher Apify plans | Once per company for which the discovery cascade completed and a non degraded row was produced, whether or not a Pinterest profile was found. Does not fire on a degraded row, because on a degraded row no discovery was performed. |
| `profile-resolved` | $0.003 on the Free plan, down to $0.0021 on higher Apify plans | Once per company whose candidate Pinterest URL passed the identity gate. Fires on the validation work, not on a populated count. A candidate dropped as an impersonator does not charge: the work was done and the honest answer is that there is no such profile. |
| `follower-count-extracted` | $0.0025 on the Free plan, down to $0.00175 on higher Apify plans | Once per company where a numeric follower count was read off the public page. Does not fire on not_extractable, blocked, identity_mismatch or url only runs. |

The [actor's Apify page](https://apify.com/mambalabs/pinterest-brand-presence-mapper) carries the current prices. Running this server consumes Apify credits.

## Full actor documentation

[apify.com/mambalabs/pinterest-brand-presence-mapper](https://apify.com/mambalabs/pinterest-brand-presence-mapper)

## Mamba Labs GTM Suite

Mamba Labs builds a fleet of GTM enrichment actors that share one flat, Clay
ready output convention, so their rows join on `company_domain` with no cleaning
step. Full fleet: [apify.com/mambalabs](https://apify.com/mambalabs)

## License

MIT
