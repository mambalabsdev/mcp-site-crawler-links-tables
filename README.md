# Site Crawler for Links and Tables MCP Server

[![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-site-crawler-links-tables/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-site-crawler-links-tables) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-site-crawler-links-tables%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-site-crawler-links-tables&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-site-crawler-links-tables)](https://www.npmjs.com/package/@mambalabsdev/mcp-site-crawler-links-tables) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-site-crawler-links-tables)](https://www.npmjs.com/package/@mambalabsdev/mcp-site-crawler-links-tables) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-site-crawler-links-tables)](https://github.com/mambalabsdev/mcp-site-crawler-links-tables/blob/main/LICENSE) [![mcpservers.org](https://img.shields.io/badge/mcpservers.org-listed-blue)](https://mcpservers.org/servers/mambalabsdev/mcp-site-crawler-links-tables)

An MCP server for the Mamba Labs Site Crawler for Links and Tables actor on Apify. Crawl a website and get its links, download links, tables, and lists as rows.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Give a start address. The actor crawls the site over plain HTTP, following links and the sitemap, and returns one row per page with internal links, external links, file download links, every HTML table as rows, and every list. The output is deterministic, with no AI. robots.txt is obeyed by default. Pages built by JavaScript return little, because no browser runs.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-site-crawler-links-tables": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-site-crawler-links-tables"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `crawl_site_links_tables` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "List every external download link on this website."
- "Pull all the tables from these pages into rows."
- "Crawl this site and show me which pages link out to other domains."

## Tool

`crawl_site_links_tables`. One tool. Example arguments:

```json
{
  "start_urls": [
    "https://webscraper.io/test-sites/tables"
  ],
  "extract": [
    "tables",
    "links"
  ],
  "max_pages": 10,
  "max_depth": 1
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `start_urls` | array | no | Web addresses to start from, up to 100. A bare domain such as example.com is read as https. |
| `max_pages` | number | no | Pages to read across the whole run, up to 10,000. Default 100. |
| `max_depth` | number | no | How many links to follow from a start address. 0 reads only the start address. Default 10. |
| `same_domain_only` | string | no | yes (default): follow links only on the start address's host and its subdomains. no: also follow links to other sites, up to Maximum pages. Links to other sites are listed either way. |
| `extract` | array | no | Any of links, tables, lists, download_links. Default links, tables, lists. download_links adds one separate row per file link, charged as download-link-row; the download_links field on page rows is always filled at no extra charge. |
| `download_extensions` | array | no | File extensions that count as download links. Default pdf, zip, xlsx, xls, docx, doc, pptx, csv, mp3, mp4, exe, dmg, msi, pkg, gz, tar, tgz, 7z, rar, iso, apk. Files are listed, never downloaded. |
| `use_sitemap` | string | no | yes (default): also queue the addresses in /sitemap.xml, which finds pages no link reaches. no: follow links only. |
| `obey_robots` | string | no | yes (default): skip pages robots.txt disallows and honor its crawl delay up to 5 seconds. no: read them, only if you have the right to. |
| `max_charge_usd` | string | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |
| `proxy_mode` | string | no | pool (default): the Apify proxy pool only, charged as page-row; a page that refuses it gets a free blocked row. auto: a refused page is retried on residential addresses, and a page read that way is charged as residential-page-row. residential: every page uses residential addresses and is charged as residential-page-row. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `page-row`, one page read on the proxy pool | $0.006 | $0.0057 | $0.0054 | $0.0051 |
| `residential-page-row`, one page read on residential addresses, only with proxy_mode auto or residential | $0.02 | $0.019 | $0.018 | $0.017 |
| `download-link-row`, one separate file link row, only when `extract` includes `download_links` | $0.0001 | $0.000095 | $0.00009 | $0.000085 |
| `apify-actor-start`, per run start | $0.0055 | $0.0055 | $0.0055 | $0.0055 |

Failed or refused pages, summary rows, pages skipped by robots.txt, and refused input are free. Mamba Labs pays the platform costs of the run, Apify proxy traffic included. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/site-crawler-links-tables](https://apify.com/mambalabs/site-crawler-links-tables)

## License

MIT. See [LICENSE](LICENSE).
