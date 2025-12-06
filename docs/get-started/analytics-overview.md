# Analytics

Rowt provides powerful server-side analytics aggregation with timezone support, filtering, and real-time data processing.

## Overview

The analytics system tracks every interaction with your links and provides aggregated insights across 13 different dimensions. All aggregation happens on the server, ensuring fast performance even with millions of interactions.

## Key Features

- **Server-Side Aggregation**: All data processing happens on the server for optimal performance
- **Timezone Support**: View analytics in your local timezone with automatic daylight saving adjustments
- **Real-Time Updates**: Data updates in real-time as interactions occur
- **Flexible Filtering**: Filter by any dimension (link, country, device, UTM parameters, etc.)
- **Automatic Granularity**: Hourly buckets for short periods, daily for longer ranges
- **Zero-Gap Time Series**: All time periods included, even those with no interactions

## Data Structure

### Interaction Properties

Every interaction captures:

| Property               | Description                                      |
|------------------------|--------------------------------------------------|
| `id`                  | Unique UUID identifier for the interaction       |
| `link`                | Reference to the clicked link                   |
| `timestamp`           | When the interaction occurred (UTC)             |
| `referer`             | Referring location where link was clicked       |
| `country`             | Country of the user (e.g., "United States")     |
| `city`                | City of the user                                |
| `ip`                  | IP address (hashed for privacy)                 |
| `device`              | Device type (e.g., "iphone", "desktop")         |
| `os`                  | Operating system (e.g., "iOS", "Windows")       |
| `browser`             | Browser name (e.g., "Safari", "Chrome")         |
| `utmSource`           | UTM source parameter                            |
| `utmMedium`           | UTM medium parameter                            |
| `utmCampaign`         | UTM campaign parameter                          |
| `utmTerm`             | UTM term parameter                              |
| `utmContent`          | UTM content parameter                           |

### Link Properties

Every link contains:

| Property               | Description                                      |
|------------------------|--------------------------------------------------|
| `id`                  | Unique short code identifier                     |
| `url`                 | Destination URL (deep link or web link)         |
| `title`               | Title for link previews (optional)              |
| `description`         | Description for link previews (optional)         |
| `imageUrl`            | Image URL for link previews (optional)           |
| `fallbackUrlOverride` | Override project's default fallback (optional)   |
| `additionalMetadata`  | Custom JSON metadata for SEO/social (optional)   |
| `properties`          | Custom properties for analytics (optional)       |
| `lifetimeClicks`      | Total number of clicks on this link             |
| `createdAt`           | When the link was created                       |

## Analytics Dimensions

The analytics endpoint aggregates data across these dimensions:

1. **Links**: Top performing links with titles and URLs
2. **Destinations**: Most visited destination URLs
3. **Countries**: Geographic distribution of clicks
4. **Cities**: City-level geographic data
5. **Devices**: Device types (iPhone, desktop, Android, iPad)
6. **Operating Systems**: OS distribution (iOS, Android, Windows, etc.)
7. **Browsers**: Browser usage (Safari, Chrome, Firefox, etc.)
8. **Referrers**: Traffic sources and referrers
9. **Link Types**: Web links vs. deep links
10. **UTM Sources**: Marketing source tracking
11. **UTM Mediums**: Marketing medium tracking
12. **UTM Campaigns**: Campaign performance tracking

Each dimension returns:
- `value`: The dimension value (e.g., "United States", "iPhone")
- `count`: Number of interactions
- `percentage`: Percentage of total interactions

## Time Series Data

Time series data shows interaction trends over time:

- **Automatic Granularity**:
  - Hourly buckets for ranges < 2 days
  - Daily buckets for ranges ≥ 2 days
- **Zero-Gap Series**: All time periods included, even with 0 interactions
- **Timezone-Aware**: Buckets aligned to your timezone, not UTC

Each data point includes:
- `timestamp`: Start of the time bucket (UTC)
- `count`: Number of interactions in that period
- `label`: Human-readable time label in your timezone (e.g., "05:00pm", "Dec 05")

## Filtering

Filter analytics by any combination of:

- **Link**: Specific link short code
- **Destination**: Destination URL
- **Link Type**: "web" or "deep"
- **Geographic**: Country, city
- **Technical**: Device, OS, browser
- **Marketing**: Referrer, UTM source/medium/campaign

Filters combine with AND logic for precise segmentation.

## Best Practices

### Timezone Handling

- Always send `startDate` and `endDate` in UTC (ISO 8601 format)
- Send user's timezone via `timezone` parameter (IANA format like "America/Chicago")
- Display using the `label` field, not the timestamp
- Server handles all timezone conversions and DST adjustments

### Performance Optimization

- Use appropriate date ranges (avoid querying years of data unnecessarily)
- Apply filters to reduce dataset size
- Adjust `topN` parameter based on UI needs (default: 10, max: 100)
- Cache results on frontend for repeated views

### Data Interpretation

- Percentages are calculated against **filtered** dataset, not all data
- Empty string values are automatically excluded from aggregations
- "Unknown" values indicate NULL or missing data
- Time series always includes current period, even if incomplete

## Example Use Cases

### Campaign Performance
```
Filter by: utmCampaign=winter_sale
View: Links, countries, devices, time series
```

### Geographic Analysis
```
Filter by: country=US
View: Cities, devices, time series
Group by timezone: America/New_York
```

### Link Comparison
```
Filter by: linkId=abc123
Compare with another query for linkId=def456
```

### Traffic Source Analysis
```
View: Referrers, UTM sources, UTM mediums
Group by: Device, OS, Browser
```

## API Reference

See [API Endpoints Documentation](/self-host/api-endpoints#analytics-endpoints) for detailed API specification.
