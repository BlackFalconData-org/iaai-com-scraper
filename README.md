# IAA Salvage Auction Scraper [💰$0.72]

Extract structured data from [iaai.com](https://iaai.com) — IAA (iaai.com) salvage and insurance auto auctions by keyword or search URL. Each vehicle row carries year, make and model with damage type, odometer and actual cash value. Branch location and auction sale time come in both local and UTC time.

**[IAA Salvage Auction Scraper [💰$0.72] on Apify →](https://apify.com/blackfalcondata/iaai-com-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword and location. Filter by 📝 description format, and more.

**Incremental mode** — Only get new or changed products since your last run. Content hash per product — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how products evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Result cap** — Stop after N products (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every product returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured products from iaai.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor products, track trends, and analyze market dynamics with structured, deduplicated data from iaai.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per product is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "toyota",
  "maxResults": 100,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all"
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | What to search for — a make, model, VIN, stock number or free text (e.g. "toyota", "2019 ford f-150"). Use a JSON array for several searches in one run. To browse all current inventory, enable Browse everything instead of leaving this empty. |
| `location` | string | — | Optional US ZIP code to centre the search on. Leave empty to search every branch nationwide. |
| `browseAll` | boolean | `false` | Return the whole current inventory, newest auctions first. Only used when no start URL, category, make or search term is given. Combine with Max Results. |
| `categories` | array | `[]` | Browse one or more vehicle categories. Combine with Makes to narrow down; Makes without a category means Cars. Replaces the search term; Start URLs take precedence over this field. |
| `makes` | array | `[]` | Vehicle makes to browse (e.g. Toyota, Ford). Applied inside each selected category; with no category, Cars is used. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Paste iaai.com pages: a search link (Search?Keyword=… with optional ZipCode/miles, or a shared Search?url=… link), a category page (Vehiclelisting/Cars/Ford) or a single lot page (VehicleDetail/46671078~US). Each URL becomes its own task; results are merged and deduped by lot across all URLs. When provided, startUrls REPLACE the category/query/location fields (a shared Search?url=… link keeps its make, ZIP code, radius and sort order; a URL with a search filter the scraper cannot apply stops the run with an input error instead of returning broader results; unsupported page types are skipped, and if none is supported the run stops with an input error). |
| `compact` | boolean | `false` | Core fields only — vehicle, VIN, damage, branch, ACV and sale date (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (queries, ZIP codes, startUrls) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip listings whose content matches an expired listing from a prior run (cross-run duplicate detection). |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit listings whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit listings that were seen before but are no longer found. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED listings. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of listings included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | Choose which representation of the listing description to include. `all` keeps every variant; the others keep only the selected one. |
| `enrichVin` | boolean | `false` | Add manufacturer, plant country/city/state, vehicle type, body class and model year decoded from the VIN with the free NHTSA vPIC service. If the service is unavailable the run still succeeds and the vin* fields are null. No extra charge. |

---

## Output fields

Every product returns the same 74-field schema. Missing values are `null` — never omitted.

- `listingId`
- `id`
- `inventoryId`
- `stockNumber`
- `lotUrl`
- `portalUrl`
- `tenant`
- `title`
- `vehicleTitle`
- `year`
- `make`
- `model`
- `series`
- `vin`
- `titleSaleDoc`
- `primaryDamage`
- `secondaryDamage`
- `lossType`
- `odometer`
- `odometerMiles`
- `odometerKm`
- `startCode`
- `airbags`
- `keys`
- `exteriorColor`
- `interiorColor`
- `engine`
- `fuelType`
- `cylinders`
- `transmission`
- `drivelineType`
- `bodyStyle`
- `countryOfOrigin`
- `vehicleType`
- `vehicleSubType`
- `location`
- `branchName`
- `branchState`
- `branchId`
- `vehicleLocation`
- `offsite`
- `laneRun`
- `aisleStall`
- `market`
- `region`
- `acv`
- `acvAmount`
- `buyNowPrice`
- `currency`
- `saleDate`
- `auctionId`
- `auctionDate`
- `auctionDateTime`
- `auctionDateTimeUtc`
- `auctionAssigned`
- `preBidAvailable`
- `buyNowAvailable`
- `timedAuction`
- `timedAuctionClosed`
- `timedAuctionCloseDateTime`
- `inventoryStatus`
- `newInventoryDate`
- `salesListUrl`
- `imageUrl`
- `requestUri`
- `searchKeyword`
- `position`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per product. Here is a real example from a production run:

```json
{
  "listingId": "e1fee315d53d2215281f11f26893275a07a0397b96d37c43bfba1e1f443471a0",
  "id": "46921942~US",
  "inventoryId": "46921942",
  "stockNumber": "46421576",
  "lotUrl": "https://www.iaai.com/VehicleDetail/46921942~US",
  "portalUrl": "https://www.iaai.com/VehicleDetail/46921942~US",
  "tenant": "US",
  "title": "2018 TOYOTA TACOMA SR5 V6",
  "vehicleTitle": "2018 TOYOTA TACOMA SR5 V6",
  "year": 2018,
  "make": "TOYOTA",
  "model": "TACOMA"
}
```

*Truncated — full records contain 74 fields. See Output fields for the complete schema.*

**[Try IAA Salvage Auction Scraper [💰$0.72] now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/iaai-com-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.00045 |
| Result | $0.00072 |

See the [actor on Apify](https://apify.com/blackfalcondata/iaai-com-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape iaai.com?**
Use this actor on Apify to extract structured data from iaai.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get iaai.com data as JSON, CSV, or Excel?**
The actor writes each product to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape iaai.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check iaai.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each product gets a content hash. On subsequent runs, only new or changed products are emitted — saving time, compute, and storage. Expired products can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

- [Bilbasen Scraper](https://apify.com/blackfalcondata/bilbasen-scraper?fpr=1h3gvi) — Denmark's largest car marketplace
- [LeBonCoin Car Scraper](https://apify.com/blackfalcondata/leboncoin-car-scraper?fpr=1h3gvi) — France's LeBonCoin car listings with make, model and vehicle specs
- [mobile.de Scraper](https://apify.com/blackfalcondata/mobile-de-scraper?fpr=1h3gvi) — Germany's largest car marketplace

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [IAA Salvage Auction Scraper [💰$0.72]](https://apify.com/blackfalcondata/iaai-com-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
