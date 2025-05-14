# create-rowt-server

The goal of this project was to create a simple, drop-in, open source package for running deeplinking.

Use the `create-rowt-server` package to scaffold a versatile API for deeplinking and analytics.

Hosting on your own infrastructure gives you more control over costs and the behavior of your API.

## Setup

```
npx create-rowt-server my-app
```

Or specify the project name when prompted:

```
npx create-rowt-server
```

1. **Create your server:**

```
npx create-rowt-server my-rowt-server
```

2. **Navigate to your project:**

```
cd my-rowt-server
```

3. **Install dependencies:**

```
npm install
```

## Configure your database
   * Set up PostgreSQL with the required `generate_uid()` function:
   
   ```sql
   CREATE OR REPLACE FUNCTION generate_uid(size INT) RETURNS TEXT AS $$
   DECLARE
     characters TEXT := 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
     bytes BYTEA := gen_random_bytes(size);
     l INT := length(characters);
     i INT := 0;
     output TEXT := '';
   BEGIN
     WHILE i < size LOOP
       output := output || substr(characters, get_byte(bytes, i) % l + 1, 1);
       i := i + 1;
     END LOOP;
     RETURN output;
   END;
   $$ LANGUAGE plpgsql VOLATILE;
   ```
   
   * Update the `DATABASE_URL` in `.env`

## Configure environment variables
   
   Create a `.env` file in your `my-rowt-server` project root:
   
   ```env
   # Database Configuration
   DATABASE_URL=postgresql://username:password@host:port/database
   
   # JWT Secret (generate a secure random string)
   JWT_SECRET=your-secure-jwt-secret-key
   
   # Server Port (optional, defaults to 3000)
   PORT=3000
   
   # Tenant Mode
   TENANT_MODE=single-tenant  # or multi-tenant
   
   # Single-Tenant Admin Credentials (only for single-tenant mode)
   ADMIN_UUID=1
   ADMIN_EMAIL=admin@example.com
   ADMIN_PASSWORD=your-secure-admin-password
   
   # Stripe Configuration (only for multi-tenant mode)
   STRIPE_SECRET_KEY=sk_test_your_stripe_key
   ```

6. **Start the server:**

```
npm run build
npm run start
```

For development mode:
```
npm run start:dev
```

## Tenant Modes
* **Single-tenant (Recommended)**: Best for individual apps or companies
  * All projects belong to one admin user
  * Requires `ADMIN_UUID`, `ADMIN_EMAIL`, and `ADMIN_PASSWORD` in `.env`
  * No Stripe integration needed
  
* **Multi-tenant**: Best for situations where you have many apps with different managers
  * Supports multiple user accounts
  * Each user can have multiple projects
  * Requires Stripe configuration for billing tiers
  * Set `STRIPE_SECRET_KEY` in `.env`

**First Steps After Setup**

**Single-Tenant Mode:**
Login with your admin credentials:
```bash
curl -X POST http://localhost:3000/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "admin@example.com",
    "password": "your-secure-admin-password"
  }'
```

**Multi-Tenant Mode:**
Create a new user account:
```bash
curl -X POST http://localhost:3000/users/create \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "secure-password",
    "role": "user"
  }'
```

### Verify Installation
* Visit `http://localhost:3000` - Should display the Rowt welcome page
* Test auth endpoint: `http://localhost:3000/auth` - Returns "Hello World!"

## Self-Hosting Your Dashboard
To manage your Rowt instance with a web interface:
* **Console SDK**: Use the [Rowt Console SDK](https://github.com/rowt-app/console-sdk) to build custom dashboards
* **Example Console**: Deploy the [Rowt Example Console](https://github.com/rowt-app/example-console) for a ready-to-use dashboard

## Production Deployment
* Use HTTPS with a reverse proxy (nginx, Caddy)
* Secure your database credentials
* Keep your JWT secret safe
* Never commit `.env` files to version control

**Documentation**
For complete setup instructions, API documentation, and guides, visit:
📚 **https://docs.rowt.app**