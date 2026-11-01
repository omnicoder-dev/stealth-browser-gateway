# Stealth Browser Gateway: anti-detect browser API for AI agents (MCP) and scraping

An [Apify Actor](https://apify.com/titan_coder/stealth-browser-gateway) that opens web pages in an anti-detect Firefox-based browser and returns clean HTML, visible text or embedded JSON (`__NEXT_DATA__`, JSON-LD). It can also run click-by-click scenarios and capture network responses. Use it from Python, JavaScript, curl, n8n, Make, LangChain, or as an MCP tool in Claude, Cursor and Windsurf.

You pay **$0.002 per successfully scraped page** ($0.02 per completed interactive scenario). Blocked and failed pages are not charged. Proxies are optional and paid by you (your Apify account or your own proxy provider).

## Measured results (1 Nov 2026, Apify cloud, no proxy, 5 attempts per site)

Success means real content came back (products, listings, price data), not just HTTP 200.

| Site | Protection | Success |
|---|---|:---:|
| Zillow listing, Walmart search, Fiverr category, Vinted catalog | PerimeterX / DataDome | 5/5 |
| Facebook Ads Library (with `successRequires`) | Meta | 5/5 |
| TikTok Explore | in-house | 3/5 |
| Upwork job search | Cloudflare | 1/5 |
| G2, Tripadvisor, Indeed | DataDome / Cloudflare | 0/5 (datacenter IPs blocked; bring a residential proxy) |

Interactive CAPTCHAs are not solved. Use public pages only and respect each site's terms.

## Use it as an MCP tool

Hosted (OAuth):

```json
{
  "mcpServers": {
    "stealth-browser-gateway": {
      "url": "https://mcp.apify.com?tools=titan_coder/stealth-browser-gateway"
    }
  }
}
```

Local (stdio):

```json
{
  "mcpServers": {
    "stealth-browser-gateway": {
      "command": "npx",
      "args": ["-y", "@apify/actors-mcp-server", "--actors", "titan_coder/stealth-browser-gateway"],
      "env": { "APIFY_TOKEN": "<YOUR_APIFY_TOKEN>" }
    }
  }
}
```

## Quick start (Python)

```python
from apify_client import ApifyClient

client = ApifyClient("<YOUR_APIFY_TOKEN>")
run = client.actor("titan_coder/stealth-browser-gateway").call(run_input={
    "startUrls": [{"url": "https://www.walmart.com/search?q=headphones"}],
    "extractMode": "html",
})
for page in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(page["url"], page["status"], page["ok"])
```

Full documentation, input options and pricing: https://apify.com/titan_coder/stealth-browser-gateway
