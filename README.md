# Pinterest Brand Presence Mapper MCP Server

[![Smithery](https://smithery.ai/badge/mambabuilt/mcp-pinterest-brand-presence-mapper)](https://smithery.ai/servers/mambabuilt/mcp-pinterest-brand-presence-mapper) [![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-pinterest-brand-presence-mapper/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-pinterest-brand-presence-mapper) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-pinterest-brand-presence-mapper)](https://www.npmjs.com/package/@mambalabsdev/mcp-pinterest-brand-presence-mapper) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-pinterest-brand-presence-mapper)](https://www.npmjs.com/package/@mambalabsdev/mcp-pinterest-brand-presence-mapper) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-pinterest-brand-presence-mapper)](https://github.com/mambalabsdev/mcp-pinterest-brand-presence-mapper/blob/main/LICENSE)

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
| `company_domain` | string | Bare company domain, for example shopify.com. Supply this or a handle. With a domain the actor runs full discovery; with a handle it skips straight to |
| `company_name` | string | Optional. Improves search accuracy and is what the identity gate checks a discovered profile against, so supplying it reduces wrong matches. |
| `handle` | string | Optional. The Pinterest username from pinterest.com/<handle>. Supplying it skips discovery and, more importantly, skips the identity risk: Pinterest h |
| `includeFollowerCounts` | boolean | When "true" (default) the profile page is fetched and the counts are extracted. Set "false" to resolve the profile URL only, which is cheaper and need |
| `skipCache` | boolean | When "false" (default) a successful lookup is cached for seven days and reused. Set "true" to force a fresh fetch. Sent as a string for Clay compatibi |

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

## Full actor documentation

[apify.com/mambalabs/pinterest-brand-presence-mapper](https://apify.com/mambalabs/pinterest-brand-presence-mapper)

## Mamba Labs GTM Suite

Mamba Labs builds a fleet of GTM enrichment actors that share one flat, Clay
ready output convention, so their rows join on `company_domain` with no cleaning
step. Full fleet: [apify.com/mambalabs](https://apify.com/mambalabs)

## License

MIT
