# Rowt Console SDK 

## Overview

The Rowt Console SDK is a TypeScript/JavaScript client library for interacting with a self-hosted Rowt server. This SDK provides a comprehensive interface for managing projects, links, users, and analytics data.

## Installation

```bash
npm install rowt-console-sdk
```

## Quick Start

```typescript
import RowtConsole from 'rowt-console-sdk';

// Initialize the client
const client = new RowtConsole('https://api.rowt.example.com');

// Login
const user = await client.login({
  email: 'user@example.com',
  password: 'password123'
});

// Get user's projects
const projects = await client.getUserProjects();
```

## Authentication

The SDK handles authentication automatically using JWT tokens with a refresh mechanism. Tokens are stored in `localStorage` and automatically refreshed when they expire.

### Login

```typescript
const user = await client.login({
  email: 'user@example.com',
  password: 'password123'
});
```

### Logout

```typescript
await client.logout();
```

### Token Management

The SDK automatically:
- Attaches access tokens to all authenticated requests
- Refreshes tokens when they expire (401 responses)
- Queues failed requests during token refresh and retries them with the new token
- Clears tokens on logout or refresh failure

## API Reference

### Initialize Client

```typescript
const client = new RowtConsole(baseURL: string);
```

- `baseURL`: The base URL of the Rowt API server

### Authentication Methods

#### login
Authenticates a user and stores tokens.

```typescript
const user = await client.login(credentials: RowtLoginDTO): Promise<RowtUser>;
```

Parameters:
- `credentials`: Object containing `email` and `password`

Returns: `RowtUser` object

#### logout
Logs out the current user and clears stored tokens.

```typescript
await client.logout(): Promise<string>;
```

Returns: Success message

#### validateUser
Validates user credentials without logging in.

```typescript
const isValid = await client.validateUser(credentials: RowtLoginDTO): Promise<boolean>;
```

Parameters:
- `credentials`: Object containing `email` and `password`

Returns: `boolean` indicating if credentials are valid

#### createUser
Creates a new user account.

```typescript
const user = await client.createUser(email: string, password: string): Promise<RowtUser>;
```

Parameters:
- `email`: User's email address
- `password`: User's password

Returns: `RowtUser` object

#### updatePassword
Updates the user's password.

```typescript
const user = await client.updatePassword(updatePasswordDTO: RowtUpdatePasswordDTO): Promise<RowtUser>;
```

Parameters:
- `updatePasswordDTO`: Object containing `email` and new `password`

Returns: Updated `RowtUser` object

### User Profile Methods

#### getProfile
Retrieves the current user's profile.

```typescript
const profile = await client.getProfile(): Promise<RowtUser>;
```

Returns: `RowtUser` object

#### getCurrentUser
Alias for getting the current user's information.

```typescript
const user = await client.getCurrentUser(): Promise<RowtUser>;
```

Returns: `RowtUser` object

### Project Management

#### getUserProjects
Retrieves all projects for the authenticated user.

```typescript
const projects = await client.getUserProjects(): Promise<RowtProject[]>;
```

Returns: Array of `RowtProject` objects

#### getProjectById
Retrieves a specific project with optional data.

```typescript
const project = await client.getProjectById(
  projectId: string, 
  options?: RowtGetProjectOptions
): Promise<RowtProject>;
```

Parameters:
- `projectId`: The project ID
- `options`: Optional configuration for including additional data
  - `includeLinks`: Include project links (default: false)
  - `includeInteractions`: Include interaction data (default: false)
  - `startDate`: Start date for analytics (default: 7 days ago)
  - `endDate`: End date for analytics (default: now)
  - `getPreviousPeriod`: Include previous period comparison data

Returns: `RowtProject` object

#### createProject
Creates a new project.

```typescript
const project = await client.createProject(project: CreateProjectDTO): Promise<RowtProject>;
```

Parameters:
- `project`: Object containing:
  - `userId`: User ID
  - `name`: Project name
  - `baseUrl`: Base URL for the project
  - `fallbackUrl`: Fallback URL

Returns: Created `RowtProject` object

#### updateProject
Updates an existing project.

```typescript
const project = await client.updateProject(project: UpdateProjectDTO): Promise<RowtProject>;
```

Parameters:
- `project`: Object containing all project fields including `id`

Returns: Updated `RowtProject` object

### Link Management

#### getLinksByProjectId
Retrieves all links for a specific project.

```typescript
const links = await client.getLinksByProjectId(
  projectId: string, 
  includeInteractions: boolean = false
): Promise<any>;
```

Parameters:
- `projectId`: The project ID
- `includeInteractions`: Whether to include interaction data (default: false)

Returns: Array of links with optional interaction data

## Type Definitions

### RowtUser
```typescript
interface RowtUser {
  id: string;
  email: string;
  role: string;
  emailVerified: boolean;
}
```

### RowtProject
```typescript
interface RowtProject {
  id: string;
  apiKey: string;
  userId: string;
  name: string;
  baseUrl: string;
  fallbackUrl: string;
  appstoreId?: string;
  playstoreId?: string;
  iosScheme?: string;
  androidScheme?: string;
  links?: RowtLink[];
  previousPeriodInteractionCount?: number;
  interactions?: RowtInteraction[];
}
```

### RowtLink
```typescript
interface RowtLink {
  id: string;
  url: string;
  title?: string;
  description?: string;
  imageUrl?: string;
  fallbackUrlOverride?: string;
  additionalMetadata?: Record<string, any>;
  properties: Record<string, any>;
  lifetimeClicks: number;
  interactions?: RowtInteraction[];
  createdAt: Date;
}
```

### RowtInteraction
```typescript
interface RowtInteraction {
  id: string;
  link: RowtLink;
  referer?: string;
  country?: string;
  device?: string;
  os?: string;
  browser?: string;
  timestamp: Date;
}
```

### RowtLoginDTO
```typescript
interface RowtLoginDTO {
  email: string;
  password: string;
}
```

### CreateProjectDTO
```typescript
interface CreateProjectDTO {
  userId: string;
  name: string;
  baseUrl: string;
  fallbackUrl: string;
}
```

### UpdateProjectDTO
```typescript
interface UpdateProjectDTO {
  id: string;
  apiKey: string;
  userId: string;
  name: string;
  baseUrl: string;
  fallbackUrl: string;
  appstoreId?: string;
  playstoreId?: string;
  iosScheme?: string;
  androidScheme?: string;
}
```

### RowtGetProjectOptions
```typescript
interface RowtGetProjectOptions {
  includeLinks?: boolean;
  includeInteractions?: boolean;
  startDate?: Date;
  endDate?: Date;
  getPreviousPeriod?: boolean;
}
```

## Error Handling

The SDK uses Axios for HTTP requests and will throw errors for failed requests. Common error scenarios:

- **401 Unauthorized**: Automatically triggers token refresh
- **Network errors**: Thrown as Axios errors
- **Invalid parameters**: Thrown as standard JavaScript errors

Example error handling:

```typescript
try {
  const user = await client.login({ email, password });
} catch (error) {
  if (error.response?.status === 401) {
    console.error('Invalid credentials');
  } else {
    console.error('Login failed:', error.message);
  }
}
```

## Examples

### Complete Authentication Flow

```typescript
import RowtConsole from 'rowt-console-sdk';

const client = new RowtConsole('https://api.rowt.example.com');

async function authenticateAndGetProjects() {
  try {
    // Login
    const user = await client.login({
      email: 'user@example.com',
      password: 'password123'
    });
    
    console.log('Logged in as:', user.email);
    
    // Get user's projects
    const projects = await client.getUserProjects();
    console.log('Projects:', projects);
    
    // Get detailed project info with analytics
    if (projects.length > 0) {
      const project = await client.getProjectById(projects[0].id, {
        includeLinks: true,
        includeInteractions: true,
        startDate: new Date(Date.now() - 30 * 24 * 60 * 60 * 1000), // 30 days ago
        endDate: new Date()
      });
      
      console.log('Project details:', project);
    }
    
    // Logout when done
    await client.logout();
    
  } catch (error) {
    console.error('Error:', error);
  }
}

authenticateAndGetProjects();
```

### Creating and Managing Projects

```typescript
async function createAndManageProject() {
  try {
    // Create a new project
    const newProject = await client.createProject({
      userId: user.id,
      name: 'My Mobile App',
      baseUrl: 'https://myapp.example.com',
      fallbackUrl: 'https://myapp.example.com/download'
    });
    
    console.log('Created project:', newProject);
    
    // Update project with app store IDs
    const updatedProject = await client.updateProject({
      ...newProject,
      appstoreId: 'com.example.myapp',
      playstoreId: 'com.example.myapp',
      iosScheme: 'myapp://',
      androidScheme: 'myapp://'
    });
    
    console.log('Updated project:', updatedProject);
    
    // Get links for the project
    const links = await client.getLinksByProjectId(updatedProject.id, true);
    console.log('Project links:', links);
    
  } catch (error) {
    console.error('Error managing project:', error);
  }
}
```

### Analytics and Reporting

```typescript
async function getProjectAnalytics(projectId: string) {
  try {
    // Get project with current period analytics
    const currentPeriod = await client.getProjectById(projectId, {
      includeInteractions: true,
      startDate: new Date(Date.now() - 7 * 24 * 60 * 60 * 1000), // Last 7 days
      endDate: new Date()
    });
    
    // Get project with previous period comparison
    const withComparison = await client.getProjectById(projectId, {
      includeInteractions: true,
      startDate: new Date(Date.now() - 7 * 24 * 60 * 60 * 1000),
      endDate: new Date(),
      getPreviousPeriod: true
    });
    
    console.log('Current period interactions:', currentPeriod.interactions?.length);
    console.log('Previous period interactions:', withComparison.previousPeriodInteractionCount);
    
    // Analyze interactions by device
    const deviceStats = currentPeriod.interactions?.reduce((acc, interaction) => {
      const device = interaction.device || 'Unknown';
      acc[device] = (acc[device] || 0) + 1;
      return acc;
    }, {} as Record<string, number>);
    
    console.log('Device breakdown:', deviceStats);
    
  } catch (error) {
    console.error('Error getting analytics:', error);
  }
}
```

## Best Practices

1. **Error Handling**: Always wrap API calls in try-catch blocks
2. **Token Storage**: The SDK uses localStorage by default; ensure your app runs in a browser environment
3. **Date Ranges**: When fetching analytics, be mindful of date ranges to avoid fetching too much data
4. **Logging Out**: Always call `logout()` when the user signs out to clear stored tokens
5. **Project IDs**: Store frequently accessed project IDs to avoid repeated lookups

## Browser Compatibility

The SDK requires:
- Modern browsers with ES6+ support
- localStorage API
- Promises/async-await support

## Contributing

This SDK is part of the Rowt platform. For contributions or issues, please refer to the main Rowt repository.

## License

[License information to be added]