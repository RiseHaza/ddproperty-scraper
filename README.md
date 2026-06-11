[Ddproperty Scraper](https://apify.com/abotapi/ddproperty-scraper?fpr=data)

# DDProperty Thailand Scraper

Extract property listings from **ddproperty.com** (PropertyGuru Thailand) at scale. Get comprehensive data including prices, features, images, agent contacts, coordinates, nearby transit (BTS/MRT), and more. Perfect for Thai real estate analytics, market research, and investment analysis.

## What does DDProperty Scraper do?

This Apify Actor scrapes property listings from ddproperty.com with built-in anti-detection. It extracts full listing data from search result pages and automatically enriches every listing with exact coordinates via the PropertyGuru Map Cluster API — no individual page visits needed.

### Key Features

| Feature | Description |
| --- | --- |
| **Anti-Detection** | Built-in stealth browser with proxy support |
| **All Listing Types** | For Sale and For Rent properties across Thailand |
| **Property Type Filters** | Condos, Houses, Villas, Townhouses, Land, Apartments, Commercial |
| **Price & Bedroom Filters** | Narrow results by price range (THB) and bedrooms |
| **Location Filters** | Filter by region (Bangkok, Phuket, Chiang Mai, Pattaya, etc.) |
| **Exact Coordinates** | Lat/lng for every listing via PropertyGuru Map Cluster API (no detail page visits) |
| **MRT/BTS Fallback** | Remaining listings enriched with nearest transit station coordinates (253 stations) |
| **Multiple Dataset Views** | Overview, Map View, Detailed, Agent Contacts |
| **Pagination** | Multi-page scraping with automatic retry on blocked pages |

## What data can you extract?

The scraper extracts 30+ fields per listing:

| **Property Details**     - Listing ID & URL - Title & full address - Property type (Condo, House, Villa, etc.) - Bedrooms & bathrooms - Floor area & land area - Tenure (Freehold/Leasehold) - Badges (New Project, Built year, etc.) | **Pricing**     - Price (THB numeric value) - Formatted price (e.g. "฿12,990,000") - Price per sqm - Listing type (Sale/Rent) |
| --- | --- |
| **Location & Transit**     - Latitude & longitude (exact, from Map Cluster API) - Region, district, area (with codes) - Nearest MRT/BTS station name & line - Distance to nearest station - Location source indicator | **Agent & Developer**     - Agent name & ID - Agency ID - Developer name - Is developer listing flag |
| **Media**     - Property images (up to 10 per listing) - Image count | **Dates & Metadata**     - Posted date (text & unix timestamp) - Recency text - Listing status (Active, etc.) |

## Quick Start

Search for condos for sale in Bangkok:

```
{
  "mode": "search",
  "listing_type": "sale",
  "property_type": "CONDO",
  "region": "TH10",
  "max_properties": 20,
  "max_pages": 3,
  "proxy": { "useApifyProxy": true }
}
```

## How to Use

### Step 1: Basic search by region

```
{
  "mode": "search",
  "listing_type": "sale",
  "region": "TH83",
  "max_properties": 50,
  "max_pages": 5,
  "proxy": { "useApifyProxy": true }
}
```

### Step 2: Filter by property type and price

```
{
  "mode": "search",
  "listing_type": "rent",
  "property_type": "CONDO",
  "region": "TH10",
  "min_price": 15000,
  "max_price": 50000,
  "bedrooms": 1,
  "max_properties": 100,
  "max_pages": 10,
  "proxy": { "useApifyProxy": true }
}
```

### Step 3: Scrape specific URLs

```
{
  "mode": "url",
  "urls": [
    "https://www.ddproperty.com/en/condo-for-sale/in-bangkok-th10",
    "https://www.ddproperty.com/en/property-for-rent/in-phuket-th83"
  ],
  "max_properties": 50,
  "max_pages": 5,
  "proxy": { "useApifyProxy": true }
}
```

## Input Parameters

| Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `mode` | string | `search` | `search` (build URL from filters) or `url` (use provided URLs) |
| `urls` | array | `[]` | DDProperty search URLs (url mode only) |
| `listing_type` | string | `sale` | `sale` or `rent` |
| `property_type` | string | - | `CONDO`, `BUNG`, `SEMI`, `VIL`, `TOWN`, `LAND`, `APT`, `RET`, `OFF`, `WAR`, `BIZ`, `SHOP` |
| `region` | string | - | Region code (see table below) |
| `min_price` | integer | - | Minimum price in THB |
| `max_price` | integer | - | Maximum price in THB |
| `bedrooms` | integer | - | Number of bedrooms (0 = Studio) |
| `sort` | string | `date` | Sort by: `date`, `price`, `psf` |
| `sort_order` | string | `desc` | `asc` or `desc` |
| `max_properties` | integer | `20` | Max listings to scrape (0 = unlimited) |
| `max_pages` | integer | `3` | Max search result pages |
| `proxy` | object | - | Proxy configuration (Apify proxy recommended) |
| `dataset_name` | string | `default` | Named dataset for results |
| `clear_dataset` | boolean | `false` | Clear dataset before scraping |

## Region Codes

| Code | Region | Code | Region |
| --- | --- | --- | --- |
| `TH10` | Bangkok | `TH83` | Phuket |
| `TH11` | Samut Prakan | `TH84` | Surat Thani (Koh Samui) |
| `TH12` | Nonthaburi | `TH77` | Prachuap Khiri Khan (Hua Hin) |
| `TH13` | Pathum Thani | `TH50` | Chiang Mai |
| `TH20` | Chon Buri (Pattaya) | `TH90` | Songkhla |

## Property Type Codes

| Code | Type | Code | Type |
| --- | --- | --- | --- |
| `CONDO` | Condo | `LAND` | Land |
| `BUNG` | Single Detached House | `APT` | Apartment |
| `SEMI` | Semi-Detached House | `RET` | Retail Space |
| `VIL` | Villa | `OFF` | Office Space |
| `TOWN` | Townhouse | `SHOP` | Shophouse |

## Output Schema

Each scraped listing is returned as a JSON object with the fields shown below. Field names and types are stable across all results; values shown are placeholder examples.

```
{
  "id": "<integer listing id>",
  "title": "<localized listing title>",
  "address": "<full property address>",
  "listing_type": "sale | rent",
  "property_type": "CONDO | BUNG | SEMI | VIL | TOWN | LAND | APT | RET | OFF | WAR | BIZ | SHOP",
  "price": "<integer THB>",
  "price_formatted": "<formatted THB string, e.g. ฿X,XXX,XXX>",
  "price_per_sqm": "<formatted THB / sqm>",
  "bedrooms": "<integer; 0 = studio>",
  "bathrooms": "<integer>",
  "floor_area": "<sqm, number>",
  "tenure": "Freehold | Leasehold",
  "developer": "<developer name, when applicable>",
  "is_developer_listing": "<boolean>",
  "agent_name": "<agent display name>",
  "agent_id": "<integer>",
  "agency_id": "<integer>",
  "region": "<region label>",
  "region_code": "<TH region code>",
  "district": "<district label>",
  "district_code": "<TH district code>",
  "area": "<area label>",
  "area_code": "<TH area code>",
  "latitude": "<float>",
  "longitude": "<float>",
  "location_source": "nearest_mrt_station | map_api",
  "nearby_mrt": "<descriptive distance to nearest MRT/BTS>",
  "nearby_station_id": "<station id, e.g. BL17>",
  "badges": ["<string>", "..."],
  "posted_date": "<DD MMM YYYY>",
  "posted_unix": "<unix timestamp>",
  "images": ["<image url>", "..."],
  "image_count": "<integer>",
  "url": "<full listing url on ddproperty.com>",
  "status": "<listing status code, e.g. ACT>"
}
```