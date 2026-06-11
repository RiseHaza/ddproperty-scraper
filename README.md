[Ddproperty Scraper](https://apify.com/fatihtahta/ddproperty-scraper?fpr=data)

# DDProperty Scraper

**Slug:** `fatihtahta/ddproperty-scraper`

## Overview

DDProperty Scraper collects structured property listing data from one or more DDProperty search result pages, including listing identity, title, address, posting date, pricing, property attributes, contact details, and media links. It is designed for teams that need repeatable extraction of publicly visible listing records into consistent JSON output for analytics, reporting, and operational workflows. [DDProperty](https://www.ddproperty.com) is a major real estate marketplace, and its public listing data is useful for market research, inventory tracking, enrichment, and recurring reporting. The actor is built for automated, repeatable collection so the same input scope can be run again over time with comparable output structure. It is well suited to scheduled data acquisition where structured records and operational consistency matter.

## Why Use This Actor

- **Market research and analytics teams:** collect structured listing, pricing, and location data for market intelligence, supply analysis, and recurring operational reporting.
- **Product and content teams:** build normalized property datasets for search experiences, editorial workflows, location pages, or internal content operations.
- **Developers and data engineering teams:** feed repeatable collection into downstream systems, ETL jobs, enrichment pipelines, and dataset normalization workflows.
- **Lead generation and enrichment teams:** capture public property and agent context that can be matched against CRM records or prospecting datasets.
- **Monitoring and competitive tracking teams:** rerun targeted searches on a schedule to observe listing movement, price changes, and segment-level coverage over time.

## Common Use Cases

- **Market intelligence:** monitor listing supply, price positioning, property mix, and geographic coverage across selected search areas.
- **Lead generation:** build targeted lists of public property listings and related agent details for enrichment or qualification workflows.
- **Competitive monitoring:** track how listing inventory changes over time for specific search segments, neighborhoods, or transaction types.
- **Catalog and directory building:** populate internal property databases with structured public records for search, review, or internal tooling.
- **Data enrichment:** append fresh public listing attributes to existing CRM, BI, or research datasets.
- **Recurring reporting:** schedule repeat runs that feed dashboards, alerts, or weekly market summaries.
- **Coverage validation:** test and compare different DDProperty search result URLs to confirm which search scopes produce the most useful datasets.

## Quick Start

1. Choose one or more DDProperty search result URLs that match the segment you want to collect.
2. Set a small `maxItems` value for the first validation run so you can confirm the output structure quickly.
3. Run the actor in Apify Console with your selected `startUrls`.
4. Review the first dataset items to confirm the records match your workflow and expected coverage.
5. Increase `maxItems`, add additional search URLs, or schedule recurring runs after the initial output is verified.

## Input Parameters

This actor accepts one or more DDProperty search result URLs and an optional maximum item cap.

| Parameter | Type | Description | Default |
| --- | --- | --- | --- |
| `startUrls` | array of strings | One or more DDProperty search result URLs to scrape. Each URL should point to a public search results page on DDProperty. | `["https://www.ddproperty.com/en/property-for-sale?freetext=phrom+phong"]` |
| `maxItems` | integer | Maximum number of property listings to return across the run. Minimum value: `1`. Leave empty to collect all available matching items within the supplied search scope. | `10` |
| `proxyConfiguration` | object | Proxy settings for the run. The default configuration uses Apify Proxy with the `RESIDENTIAL` group. | `{"useApifyProxy": true, "apifyProxyGroups": ["RESIDENTIAL"]}` |

## Choosing Inputs

Start with `startUrls`, because those URLs define the exact public search scope the actor will collect. Narrower search result URLs usually produce more targeted datasets, while broader search URLs are better for discovery and higher-volume collection. If you are validating a new workflow, begin with a small `maxItems` value and inspect the first records before increasing coverage. When you need multiple segments, use multiple `startUrls` so each run captures a clear and intentional scope. Keep the default proxy configuration unless you have a specific reason to change connection settings in your Apify environment.

## Example Inputs

### Scenario: Condo sale search with a conservative validation limit

```
{
  "startUrls": [
    "https://www.ddproperty.com/en/property-for-sale?freetext=phrom+phong"
  ],
  "maxItems": 20,
  "proxyConfiguration": {
    "useApifyProxy": true,
    "apifyProxyGroups": ["RESIDENTIAL"]
  }
}
```

### Scenario: Rental search using a direct DDProperty results URL

```
{
  "startUrls": [
    "https://www.ddproperty.com/en/property-for-rent?listingType=rent&isCommercial=false&freetext=thong+lor"
  ],
  "maxItems": 50,
  "proxyConfiguration": {
    "useApifyProxy": true,
    "apifyProxyGroups": ["RESIDENTIAL"]
  }
}
```

### Scenario: Broad discovery run across multiple search scopes

```
{
  "startUrls": [
    "https://www.ddproperty.com/en/property-for-sale?freetext=bang+na",
    "https://www.ddproperty.com/en/property-for-sale?freetext=ari"
  ],
  "maxItems": 100,
  "proxyConfiguration": {
    "useApifyProxy": true,
    "apifyProxyGroups": ["RESIDENTIAL"]
  }
}
```

## Output

### 9.1 Output destination

The actor writes results to an Apify dataset as JSON records. The dataset is designed for direct consumption by analytics tools, ETL pipelines, and downstream APIs without post-processing.

Each item contains a stable record envelope plus a type-specific payload. For this actor, the payload represents a property listing record from DDProperty search results.

### 9.2 Record envelope (all items)

Every record includes the following stable identifiers:

- **`type`** *(string, required)*: record type. For this actor, records use `listing`.
- **`id`** *(number, required)*: stable numeric identifier for the record.
- **`url`** *(string, required)*: canonical public URL for the record.

Recommended idempotency key: `type + ":" + id`

Use the idempotency key for deduplication and upserts when loading repeated runs into databases, warehouses, CRMs, or search indexes. The stable envelope makes records easier to merge, deduplicate, and sync across repeated runs.

### 9.3 Examples

Example: listing (`type = "listing"`)

```
{
  "type": "listing",
  "id": 500249115,
  "url": "https://www.ddproperty.com/en/property/the-diplomat-39-for-rent-500249115",
  "listing_id": 500249115,
  "title": "The Diplomat 39, Bangkok",
  "address": "18 Soi Sukhumvit 39, Sukhumvit Road, Khlong Tan Nua, Watthana, Bangkok",
  "posted_on": "12 Apr 2026",
  "search_page_number": 1,
  "source_context": {
    "listing_url": "https://www.ddproperty.com/en/property/the-diplomat-39-for-rent-500249115",
    "search_page_url": "https://www.ddproperty.com/en/property-for-rent?listingType=rent&page=1&isCommercial=false&freetext=phrom+phong",
    "search_seed_url": "https://www.ddproperty.com/en/property-for-rent?listingType=rent&isCommercial=false&freetext=phrom+phong",
    "scraped_at": "2026-04-26T11:26:17.460Z"
  },
  "pricing": {
    "amount": 55000,
    "currency": "THB",
    "price_per_sqm": 982.14,
    "price_per_sqm_text": "฿982.14 / sqm"
  },
  "property_details": {
    "listing_type": "For Rent",
    "property_type": "Condo",
    "bedrooms": -1,
    "bathrooms": 0,
    "floor_area_sqm": 56
  },
  "contact": {
    "agent_name": "Cozy Agent Property",
    "agent_profile_url": "https://www.ddproperty.com/en/agent/cozy-agent-property-900445323",
    "developer_name": null
  },
  "media": {
    "thumbnail_url": "https://th1-cdn.pgimgs.com/listing/500249115/UPHO.135437691.V800/The-Diplomat-39-Watthana-Thailand.jpg",
    "image_urls": [
      "https://th1-cdn.pgimgs.com/listing/500249115/UPHO.135437691.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://th1-cdn.pgimgs.com/listing/500249115/UPHO.135437692.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://th1-cdn.pgimgs.com/listing/500249115/UPHO.135437693.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://th1-cdn.pgimgs.com/listing/500249115/UPHO.135437694.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://th1-cdn.pgimgs.com/listing/500249115/UPHO.135437695.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://th1-cdn.pgimgs.com/listing/500249115/UPHO.135437696.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://th1-cdn.pgimgs.com/listing/500249115/UPHO.135437697.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://th1-cdn.pgimgs.com/listing/500249115/UPHO.135437698.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://th1-cdn.pgimgs.com/listing/500249115/UPHO.135437699.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://th1-cdn.pgimgs.com/listing/500249115/UPHO.135437700.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://th1-cdn.pgimgs.com/listing/500249115/UPHO.135437701.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://th1-cdn.pgimgs.com/listing/500249115/UPHO.135437702.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://sg1-cdn.pgimgs.com/projectnet-project/59969/ZPPHO.102018707.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://sg1-cdn.pgimgs.com/projectnet-project/59969/ZPPHO.102018708.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://sg1-cdn.pgimgs.com/projectnet-project/59969/ZPPHO.102018709.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://sg1-cdn.pgimgs.com/projectnet-project/59969/ZPPHO.102018710.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://sg1-cdn.pgimgs.com/projectnet-project/59969/ZPPHO.102018711.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://sg1-cdn.pgimgs.com/projectnet-project/59969/ZPPHO.102018713.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://sg1-cdn.pgimgs.com/projectnet-project/59969/ZPPHO.102018714.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://sg1-cdn.pgimgs.com/projectnet-project/59969/ZPPHO.102018715.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://sg1-cdn.pgimgs.com/projectnet-project/59969/ZPPHO.103739073.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://sg1-cdn.pgimgs.com/projectnet-project/59969/ZPPHO.103739074.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://sg1-cdn.pgimgs.com/projectnet-project/59969/ZPPHO.103739076.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://sg1-cdn.pgimgs.com/projectnet-project/59969/ZPPHO.103739078.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://sg1-cdn.pgimgs.com/projectnet-project/59969/ZPPHO.103739079.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://sg1-cdn.pgimgs.com/projectnet-project/59969/ZPPHO.103739080.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://sg1-cdn.pgimgs.com/projectnet-project/59969/ZPPHO.103739081.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://sg1-cdn.pgimgs.com/projectnet-project/59969/ZPPHO.103739082.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://sg1-cdn.pgimgs.com/projectnet-project/59969/ZPPHO.103739083.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://sg1-cdn.pgimgs.com/projectnet-project/59969/ZPPHO.103739085.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://sg1-cdn.pgimgs.com/projectnet-project/59969/ZPPHO.103739087.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://sg1-cdn.pgimgs.com/projectnet-project/59969/ZPPHO.103739088.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://sg1-cdn.pgimgs.com/projectnet-project/59969/ZPPHO.103739089.V800/The-Diplomat-39-Watthana-Thailand.jpg",
      "https://sg1-cdn.pgimgs.com/projectnet-project/59969/ZPPHO.103739091.V800/The-Diplomat-39-Watthana-Thailand.jpg"
    ],
    "video_embed_html": []
  }
}
```

## Field Reference

### Record type: `listing`

- **`type`** *(string, required)*: Stable record type. Value is `listing`.
- **`id`** *(number, required)*: Stable numeric identifier for the listing.
- **`url`** *(string, required)*: Canonical public URL for the listing.
- **`listing_id`** *(number, required)*: Source listing identifier from DDProperty.
- **`title`** *(string, required)*: Listing title.
- **`address`** *(string, optional)*: Public address text shown for the listing.
- **`posted_on`** *(string, optional)*: Public posting date as displayed on the source site.
- **`search_page_number`** *(number, optional)*: Search results page number where the item was captured.
- **`source_context.listing_url`** *(string, required)*: Listing URL captured during the run.
- **`source_context.search_page_url`** *(string, required)*: Search results page URL where the record was found.
- **`source_context.search_seed_url`** *(string, required)*: Original input search URL associated with the record.
- **`source_context.scraped_at`** *(string, required)*: UTC timestamp showing when the record was collected.
- **`pricing.amount`** *(number, optional)*: Listing price amount in the stated currency.
- **`pricing.currency`** *(string, optional)*: Currency code or currency label for the listing price, such as `THB`.
- **`pricing.price_per_sqm`** *(number, optional)*: Price per square meter.
- **`pricing.price_per_sqm_text`** *(string, optional)*: Display-ready price-per-area text.
- **`property_details.listing_type`** *(string, optional)*: Transaction type, such as `For Rent` or `For Sale`.
- **`property_details.property_type`** *(string, optional)*: Property category, such as condo, house, or townhouse.
- **`property_details.bedrooms`** *(number, optional)*: Bedroom count when provided.
- **`property_details.bathrooms`** *(number, optional)*: Bathroom count when provided.
- **`property_details.floor_area_sqm`** *(number, optional)*: Floor area in square meters.
- **`contact.agent_name`** *(string, optional)*: Public agent or representative name.
- **`contact.agent_profile_url`** *(string, optional)*: Public URL of the associated agent profile.
- **`contact.developer_name`** *(string, optional)*: Public developer name when available.
- **`media.thumbnail_url`** *(string, optional)*: Primary thumbnail image URL.
- **`media.image_urls`** *(array of strings, optional)*: Full set of collected listing image URLs.
- **`media.video_embed_html`** *(array of strings, optional)*: Embedded video markup when provided on the public listing.

## Data Quality, Guarantees, And Handling

- **Structured records:** results are normalized into predictable JSON objects for downstream use.
- **Best-effort extraction:** fields may vary by region, session, availability, or source-side presentation changes.
- **Optional fields:** null-check in downstream code.
- **Deduplication:** recommend `type + ":" + id`.
- **Freshness:** results reflect the publicly available data at run time.
- **Repeated runs:** use the recommended idempotency key when syncing data into warehouses, CRMs, or search indexes.

## Tips For Best Results

- Start with a small `maxItems` value to validate output shape and field coverage before scaling up.
- Use one search segment or one closely related group of `startUrls` per run when you need cleaner dataset segmentation.
- Leave `maxItems` empty only when you want the full available volume for the supplied search scope.
- Add additional search URLs gradually so you can understand how each scope affects coverage and output mix.
- Reuse stable DDProperty search result URLs for recurring monitoring workflows.
- Keep the recommended `type + ":" + id` key in downstream storage to simplify deduplication across repeated runs.
- Inspect a few early dataset records before scheduling large recurring runs.

## How to Run on Apify

1. Open the actor in Apify Console.
2. Configure the available input fields for the target search scope.
3. Set the maximum number of outputs to collect with `maxItems`, if needed.
4. Click **Start** and wait for the run to finish.
5. Download results in JSON, CSV, Excel, or other supported dataset formats.

## Scheduling & Automation

### Scheduling

**Automated Data Collection**

You can schedule runs to keep your DDProperty dataset current without manual repetition. Scheduled runs are useful for recurring market tracking, reporting workflows, and routine data refreshes.

- Navigate to **Schedules** in Apify Console
- Create a new schedule (daily, weekly, or custom cron)
- Configure input parameters
- Enable notifications for run completion
- Add webhooks for automated processing

### Integration Options

- **BI dashboards:** monitor pricing, listing volume, and geographic coverage trends over time.
- **Data warehouses:** load normalized listing records into historical reporting and analytics models.
- **Webhooks:** trigger ingestion, validation, or downstream update workflows after each completed run.
- **CRM enrichment:** attach public property and agent context to account, lead, or opportunity records where relevant.
- **Google Sheets or Airtable:** review smaller validation runs, share listings with non-technical teams, or manage lightweight operational workflows.
- **ETL and enrichment pipelines:** merge listing data with internal market, sales, or research datasets for broader analysis.

## Export Formats And Downstream Use

Apify datasets can be exported directly or consumed programmatically by downstream systems. This makes the actor suitable for both manual review and automated production workflows.

- **JSON:** for APIs, applications, and data pipelines.
- **CSV or Excel:** for spreadsheet workflows and manual review.
- **API access:** for automated ingestion into internal systems.
- **BI and warehouses:** for reporting, dashboards, and historical analysis.

## Performance

Estimated run times:

- **Small runs (< 1,000 outputs):** ~3–5 minutes
- **Medium runs (1,000–5,000 outputs):** ~5–15 minutes
- **Large runs (5,000+ outputs):** ~15–30 minutes

Execution time varies based on search scope, result volume, and how much information is returned per record. Highly filtered runs can finish faster, while broad discovery or detail-rich records may take longer.

## Limitations

- Availability depends on what [DDProperty](https://www.ddproperty.com) publicly exposes at run time.
- Some optional fields may be missing on sparse listings or records with limited public detail.
- Very broad searches can take longer and may require higher `maxItems` values to reach the desired volume.
- Public field availability or naming can change when the source site updates listing presentation.
- Regional, listing-type, or account-level differences may affect which records and fields are visible.

## Troubleshooting

- **No results returned:** check that the `startUrls` point to valid public DDProperty search result pages and that the search scope contains matching listings.
- **Fewer results than expected:** broaden the search scope, increase `maxItems`, or verify that the supplied DDProperty results pages contain enough matching records.
- **Some fields are empty:** optional fields depend on what each listing publicly provides.
- **Run takes longer than expected:** reduce scope, lower `maxItems` for validation, or split broad collection into smaller search segments.
- **Output changed:** compare the current results with the field reference and include a small sample if you need support.

## FAQ

### What data does this actor collect?

It collects structured property listing data from DDProperty search result pages, including identifiers, URLs, titles, addresses, dates, pricing, property details, contact fields, and media links when publicly available.

### Can I filter by location, category, date, price, or other criteria?

The actor follows the DDProperty search result URLs you provide in `startUrls`. Any targeting supported by those public search URLs can be reflected in the dataset by choosing the right search pages.

### Why did I receive fewer results than my limit?

`maxItems` sets an upper bound, not a guaranteed volume. If the supplied search pages contain fewer public listings than your limit, the dataset will contain fewer items.

### Can I schedule recurring runs?

Yes. Apify scheduling can run the actor on a daily, weekly, or custom cadence so your dataset stays current.

### How do I avoid duplicates across runs?

Use the recommended idempotency key `type + ":" + id` when loading records into downstream systems.

### Can I export the data to CSV, Excel, or JSON?

Yes. Apify datasets support JSON export as well as spreadsheet-friendly formats such as CSV and Excel.

### Does this actor collect private data?

No. It is intended for publicly available listing information from DDProperty.

### What should I include when reporting an issue?

Include the input used, with sensitive values redacted if necessary, the Apify run ID, expected versus actual behavior, and a small output sample when possible.

## Compliance & Ethics

### Responsible Data Collection

This actor collects publicly available real estate listing information from [DDProperty](https://www.ddproperty.com) for legitimate business purposes, including:

- **Real estate** research and market analysis
- **Listing monitoring and operational reporting**
- **Dataset enrichment and business intelligence workflows**

Users are responsible for ensuring their use of the actor and any collected data complies with applicable laws, regulations, and the target site's terms. This section is informational and not legal advice.

### Best Practices

- Use collected data in accordance with applicable laws, regulations, and the target site’s terms
- Respect individual privacy and personal information
- Use data responsibly and avoid disruptive or excessive collection
- Do not use this actor for spamming, harassment, or other harmful purposes
- Follow relevant data protection requirements where applicable, including GDPR and CCPA

## Support

For help, use the actor page or the repository Issues section. When reporting a problem, include the input used with any sensitive details redacted, the run ID, the expected behavior, the actual behavior, and an optional small output sample that illustrates the issue.