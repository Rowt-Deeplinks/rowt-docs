# Including Additional Metadata in Rowt Links

When creating dynamic links with the Rowt SDK, you can include custom metadata using the `additionalMetadata` field. This field accepts any JSON object and is stored as a PostgreSQL JSONB column on the backend.

## How to Use Additional Metadata

The `additionalMetadata` field is a flexible key-value store that can contain any JSON-serializable data you need to associate with your link:

```typescript
import { RowtLink } from 'rowt-sdk';

const config = {
  serverUrl: 'https://your-server.com',
  apiKey: 'your-api-key',
  projectId: 'your-project-id'
};

const linkOptions = {
  url: 'myapp://products/123',
  title: 'Summer Sale Product',
  // Include any custom JSON data in additionalMetadata
  additionalMetadata: {
    campaign: {
      name: 'summer-2024',
      channel: 'email',
      variant: 'A'
    },
    analytics: {
      source: 'newsletter',
      medium: 'email', 
      content: 'hero-banner'
    },
    product: {
      sku: 'PROD-123',
      category: 'electronics',
      price: 299.99,
      currency: 'USD'
    },
    user: {
      segmentId: 'high-value',
      cohort: '2024-Q2'
    },
    // Arrays are supported
    tags: ['featured', 'sale', 'limited-time'],
    // Nested objects are supported
    customData: {
      experimentId: 'exp-456',
      metadata: {
        version: 'v2',
        timestamp: new Date().toISOString()
      }
    }
  }
};

const rowtLink = new RowtLink(config, linkOptions);
const shortLink = await rowtLink.createLink();
```

## Common Use Cases for Additional Metadata

### 1. Analytics Tracking

```typescript
additionalMetadata: {
  utm_source: 'facebook',
  utm_medium: 'social',
  utm_campaign: 'summer-sale-2024',
  utm_content: 'carousel-ad-1',
  utm_term: 'discount-electronics'
}
```

### 2. A/B Testing

```typescript
additionalMetadata: {
  experiment: {
    id: 'pricing-test-001',
    variant: 'B',
    group: 'treatment'
  }
}
```

### 3. User Attribution

```typescript
additionalMetadata: {
  referrer: {
    userId: 'user-789',
    referralCode: 'FRIEND2024',
    tier: 'gold'
  },
  attribution: {
    firstTouch: 'google-ads',
    lastTouch: 'email'
  }
}
```

### 4. Content Metadata

```typescript
additionalMetadata: {
  content: {
    type: 'video',
    duration: 180,
    format: 'mp4',
    thumbnail: 'https://example.com/thumb.jpg'
  },
  localization: {
    language: 'en-US',
    region: 'north-america'
  }
}
```

### 5. Business Logic

```typescript
additionalMetadata: {
  promotion: {
    code: 'SAVE20',
    validUntil: '2024-12-31',
    minimumPurchase: 50
  },
  inventory: {
    stock: 145,
    warehouse: 'east-coast',
    reserved: 12
  }
}
```

## Important Notes

- The `additionalMetadata` field is stored as PostgreSQL JSONB, which provides efficient querying and indexing capabilities
- The field accepts any valid JSON structure (objects, arrays, strings, numbers, booleans, null)
- There's no strict schema enforcement, giving you flexibility to include whatever data you need
- The default value is an empty object `{}` if no metadata is provided
- All metadata is preserved exactly as provided and can be retrieved later through your backend API

## TypeORM Schema

The backend enforces this structure using TypeORM:

```typescript
@Column({ type: 'jsonb', default: {} }) // JSONB with default value
additionalMetadata?: Record<string, any>;
```

## Best Practices

1. **Keep it structured**: While the field is flexible, maintain consistent structure for similar link types
2. **Use meaningful keys**: Choose descriptive key names that clearly indicate the data's purpose
3. **Avoid sensitive data**: Don't include passwords, API keys, or other sensitive information
4. **Consider size**: While JSONB can handle large objects, keep metadata reasonably sized for performance
5. **Document your schema**: Even though it's flexible, document what metadata fields your application uses

## Example: Complete Link Creation with Rich Metadata

```typescript
import { RowtLink } from 'rowt-sdk';

async function createRichLink() {
  const config = {
    serverUrl: 'https://api.rowt.io',
    apiKey: 'your-api-key',
    projectId: 'your-project-id'
  };
  
  const linkOptions = {
    url: 'myapp://special-offer',
    title: 'Exclusive Black Friday Deal',
    description: 'Save 50% on all electronics',
    imageUrl: 'https://cdn.example.com/black-friday.jpg',
    expiration: new Date('2024-11-30'),
    additionalMetadata: {
      // Campaign tracking
      campaign: {
        id: 'bf-2024',
        type: 'seasonal',
        budget: 50000,
        department: 'marketing'
      },
      // Target audience
      targeting: {
        segments: ['high-value', 'electronics-interested'],
        geoTargets: ['US', 'CA'],
        ageRange: { min: 25, max: 54 }
      },
      // Performance tracking
      performance: {
        expectedCTR: 0.05,
        conversionGoal: 1000,
        revenueTarget: 150000
      },
      // Feature flags
      features: {
        showCountdown: true,
        enableChat: false,
        personalizedOffers: true
      },
      // Timestamps and versioning
      metadata: {
        createdBy: 'marketing-team',
        createdAt: new Date().toISOString(),
        version: '2.1',
        environment: 'production'
      }
    },
    // Regular properties (non-metadata)
    properties: {
      source: 'email-campaign'
    }
  };
  
  const rowtLink = new RowtLink(config, linkOptions);
  const shortLink = await rowtLink.createLink();
  
  console.log('Created link:', shortLink);
  console.log('Link shortcode:', rowtLink.getShortcode());
  
  return rowtLink;
}
```

This flexible metadata system allows you to attach any contextual information to your links, making them powerful tools for tracking, analytics, personalization, and business logic.