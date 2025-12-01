# Migrating to Version 1.0 from Beta

If you're upgrading from a beta version of Rowt Server to version 1.0, you'll need to update your database schema to include new fields for improved analytics and observability.

## What's New in 1.0

### Enhanced Analytics
- **IP Address Storage** - Track visitor IPs for better repeat visitor identification
- **City-Level Geolocation** - Cloudflare header support for city-level data
- **UTM Parameter Tracking** - Full support for utm_source, utm_medium, utm_campaign, utm_term, and utm_content
- **Improved Country Detection** - Cloudflare CF-IPCountry header support with fallback to timezone detection

### Better Observability
- **Timestamps on Users** - Track when users were created and last updated
- **Timestamps on Projects** - Track project creation and modification times
- **Structured Logging** - JSON-formatted logs for better searching and filtering

### New Features
- **Custom Shortcodes** - Create memorable branded links (1-12 characters)
- **HTTPS Enforcement** - Automatic HTTP to HTTPS redirect in production
- **Docker Support** - Production-ready Dockerfile included

## Database Migration

You have two options for migrating your database:

### Option 1: Using TypeORM Migrations (Recommended)

This approach creates a migration file that's tracked in version control.

```bash
# Generate the migration
npm run migration:generate -- src/database/migrations/UpgradeToV1

# Review the generated migration file in src/database/migrations/

# Run the migration
npm run migration:run
```

### Option 2: Manual SQL Migration

If you prefer to run SQL directly, use the appropriate commands for your database.

#### PostgreSQL

```sql
-- Add timestamps to users table
ALTER TABLE users
ADD COLUMN created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
ADD COLUMN updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP;

-- Add timestamps to projects table
ALTER TABLE projects
ADD COLUMN created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
ADD COLUMN updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP;

-- Add analytics columns to interactions table
ALTER TABLE interactions
ADD COLUMN city VARCHAR(255),
ADD COLUMN ip VARCHAR(45),
ADD COLUMN utm_source VARCHAR(255),
ADD COLUMN utm_medium VARCHAR(255),
ADD COLUMN utm_campaign VARCHAR(255),
ADD COLUMN utm_term VARCHAR(255),
ADD COLUMN utm_content VARCHAR(255);
```

#### SQLite

```sql
-- SQLite doesn't support multiple ADD COLUMN in one statement
ALTER TABLE users ADD COLUMN created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP;
ALTER TABLE users ADD COLUMN updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP;

ALTER TABLE projects ADD COLUMN created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP;
ALTER TABLE projects ADD COLUMN updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP;

ALTER TABLE interactions ADD COLUMN city VARCHAR(255);
ALTER TABLE interactions ADD COLUMN ip VARCHAR(45);
ALTER TABLE interactions ADD COLUMN utm_source VARCHAR(255);
ALTER TABLE interactions ADD COLUMN utm_medium VARCHAR(255);
ALTER TABLE interactions ADD COLUMN utm_campaign VARCHAR(255);
ALTER TABLE interactions ADD COLUMN utm_term VARCHAR(255);
ALTER TABLE interactions ADD COLUMN utm_content VARCHAR(255);
```

## Post-Migration

After running the migration, restart your Rowt server:

```bash
npm run start:prod
```

## Cloudflare Setup (Optional)

To take advantage of improved geolocation with Cloudflare:

1. Set up your domain with Cloudflare DNS
2. Enable the Cloudflare proxy (orange cloud icon)
3. Point your domain to your Rowt server

Rowt will automatically detect and use Cloudflare headers when available. If not using Cloudflare, the server falls back to timezone-based country detection.

## UTM Parameter Usage

You can now track campaign data by adding UTM parameters to your short links:

```
https://yourdomain.com/abc123?utm_source=instagram&utm_medium=social&utm_campaign=summer_sale
```

These parameters are automatically extracted and stored in the analytics database.

## Custom Shortcodes

Create branded, memorable links:

```typescript
const myLink = new RowtLink(config, {
    url: 'https://example.com',
    customShortcode: 'summer-sale' // Creates yourdomain.com/summer-sale
})
```

See the [Creating Links](/app-sdk/creating-links#custom-shortcodes) documentation for more details.

## Need Help?

If you encounter issues during migration:

1. Check that you're running Node 20+
2. Verify your database connection is working
3. Review the migration logs for errors
4. Report issues at [github.com/anthropics/rowt-server/issues](https://github.com/anthropics/rowt-server/issues)
