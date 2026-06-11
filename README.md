[Ddproperty Scraper](https://apify.com/shahidirfan/ddproperty-scraper?fpr=data)

Extract DDProperty property listings for sale or rent from search pages and save clean, analysis-ready records to your dataset.

---

## Features

- Collects property listings from DDProperty sale and rent search pages.
- Preserves stable collection across multiple pages with duplicate-safe output.
- Captures rich listing details for pricing, property specs, location, agent metadata, and verification flags.
- Uses adaptive access flow to keep extraction running when one access path is challenged.
- Produces clean, analysis-ready records with null/empty fields removed.

---

## Use Cases

- Property market monitoring
- Lead generation research
- Price trend analysis
- Competitor listing intelligence
- Real estate dashboard pipelines

---

## Input Parameters

| Field | Type | Description |
| --- | --- | --- |
| `startUrl` | string | DDProperty search URL to start from |
| `listingType` | string | `sale` or `rent` |
| `isCommercial` | boolean | Toggle commercial search context |
| `results_wanted` | integer | Max number of records to save |
| `max_pages` | integer | Max number of pages to crawl |
| `proxyConfiguration` | object | Proxy settings (residential recommended) |

---

## Output Data

| Field | Type | Description |
| --- | --- | --- |
| `id` | string | Listing ID |
| `externalId` | string | External listing ID |
| `title` | string | Listing title |
| `url` | string | Listing URL |
| `statusCode` | string | Listing status code |
| `price` | number | Listing numeric price |
| `priceText` | string | Formatted listing price |
| `pricePerArea` | string | Price per sqm text |
| `currency` | string | Currency |
| `listingType` | string | Sale or rent |
| `propertyType` | string | Property type |
| `bedrooms` | number | Bedroom count |
| `bathrooms` | number | Bathroom count |
| `floorAreaSqm` | number | Floor area (sqm) |
| `landAreaSqm` | number | Land area (sqm) |
| `district` | string | District name |
| `region` | string | Region |
| `address` | string | Address text |
| `projectName` | string | Project or development name |
| `agentName` | string | Agent name |
| `agentId` | number | Agent ID |
| `agentVerified` | boolean | Agent verification flag |
| `imageUrl` | string | Main image URL |
| `isVerified` | boolean | Listing verification flag |
| `isOfficialListing` | boolean | Official listing indicator |
| `isDeveloperListing` | boolean | Developer listing indicator |
| `accountTypeCode` | string | Listing account type code |
| `recencyText` | string | Posted/reposted recency text |
| `postedOnText` | string | Display posted date |
| `postedOnUnix` | number | Unix timestamp for posting date |
| `sourceUrl` | string | Source search page URL |
| `page` | number | Source page number |
| `scrapedAt` | string | ISO timestamp |

---

## Usage Examples

```
{
  "startUrl": "https://www.ddproperty.com/en/property-for-sale?listingType=sale&isCommercial=false",
  "listingType": "sale",
  "isCommercial": false,
  "results_wanted": 20,
  "max_pages": 3,
  "proxyConfiguration": {
    "useApifyProxy": true,
    "apifyProxyGroups": ["RESIDENTIAL"]
  }
}
```

---

## Sample Output

```
{
  "id": "12345678",
  "title": "Condo in Bangkok",
  "url": "https://www.ddproperty.com/en/property/example-12345678",
  "statusCode": "ACT",
  "price": 8500000,
  "priceText": "฿8,500,000",
  "pricePerArea": "฿94,444 / sqm",
  "currency": "THB",
  "listingType": "For Sale",
  "propertyType": "Condo",
  "bedrooms": 3,
  "bathrooms": 2,
  "floorAreaSqm": 95,
  "landAreaSqm": 110,
  "projectName": "Example Residence",
  "district": "Bang Na",
  "region": "Bangkok",
  "agentName": "Example Agent",
  "agentId": 81234,
  "agentVerified": true,
  "isVerified": true,
  "isOfficialListing": false,
  "isDeveloperListing": false,
  "accountTypeCode": "agent",
  "postedOnText": "2 days ago",
  "postedOnUnix": 1772755200,
  "sourceUrl": "https://www.ddproperty.com/en/property-for-sale?listingType=sale&isCommercial=false",
  "page": 1,
  "scrapedAt": "2026-03-05T12:00:00.000Z"
}
```

---

## Tips

- Keep `results_wanted` close to your real need to reduce run time.
- Use residential proxies for better continuity on protected traffic paths.
- Increase `max_pages` gradually for large crawls.
- If results are low, rerun with residential proxies and a lower page limit first.

---

## Integrations

- Schedule runs for continuous monitoring
- Connect dataset output to webhooks
- Export to CSV/JSON for BI workflows
- Chain with downstream automations in your data pipeline

---

## FAQ

**Q: Why do I get few or zero records?**

A: The site may be blocking your current network path. Use residential proxies and keep concurrency low.

**Q: Can I run sale and rent separately?**

A: Yes. Set `listingType` to `sale` or `rent`.

## Legal Notice

Use this actor responsibly and in compliance with DDProperty terms, robots policies, and applicable laws. You are responsible for lawful use of collected data.