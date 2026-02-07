# API Configuration Guide

This guide explains how to configure the Aira API backend for local development with the `aira-client-os` frontend.

## What is the Aira API?

### Base URL
```
https://dev.api.airaai.in
```

### Purpose & Overview
The Aira API is the backend service that powers the Aira frontend dashboard. It provides:
- User authentication and authorization
- Connector management (WhatsApp, Email, Calendar, etc.)
- Rule configuration and execution
- Task management
- Real-time data synchronization

The API follows RESTful conventions and is the primary data source for the Next.js web dashboard at [app.airaai.in](https://app.airaai.in).

## Setting Up for Local Development

### Step 1: Create Environment Configuration

Navigate to the web app directory and create your local environment file:

```bash
cd apps/aira-web
cp .env.example .env.local
```

### Step 2: Configure the API Base URL

Open `.env.local` and set the `NEXT_PUBLIC_API_BASE_URL`:

```bash
# API Configuration
NEXT_PUBLIC_API_BASE_URL=https://dev.api.airaai.in
NEXT_PUBLIC_API_TIMEOUT=30000

# Google OAuth
NEXT_PUBLIC_GOOGLE_AUTH_URL=https://dev.api.airaai.in/auth/google
```

> **Important:** The `NEXT_PUBLIC_` prefix makes these variables accessible in the browser. Never expose secrets using this prefix.

### Step 3: Start the Development Server

From the project root:

```bash
pnpm install
pnpm dev --filter=aira-web
```

The web app will be available at [http://localhost:3000](http://localhost:3000).

## Example .env.local File

Here's a complete example of `apps/aira-web/.env.local`:

```bash
# API Configuration
NEXT_PUBLIC_API_BASE_URL=https://dev.api.airaai.in
NEXT_PUBLIC_API_TIMEOUT=30000

# Google OAuth
NEXT_PUBLIC_GOOGLE_AUTH_URL=https://dev.api.airaai.in/auth/google
```

## How the API Client Works

The frontend uses the configured base URL to initialize the API client:

- **Location:** `apps/aira-web/src/lib/api.ts`
- **Client:** Axios-based client from `@repo/core`
- **Features:**
  - Automatic JWT token handling
  - Request/response interceptors
  - Type-safe API calls with Zod validation
  - Cookie-based authentication

## Using the OpenAPI Specification

### What is OpenAPI?

If you have an OpenAPI specification file (usually `openapi.yaml` or `openapi.json`) for the Aira API, you can use it to:
- Generate type-safe API clients
- Browse interactive API documentation
- Test API endpoints locally

### Viewing with Swagger UI

1. Install Swagger UI locally:
   ```bash
   npx swagger-ui-watcher openapi.yaml
   ```

2. Or use the online editor:
   - Visit [editor.swagger.io](https://editor.swagger.io)
   - Paste your OpenAPI spec content
   - Browse endpoints and schemas interactively

### Viewing with Redoc

1. Install Redoc CLI:
   ```bash
   npm install -g redoc-cli
   ```

2. Serve the documentation:
   ```bash
   redoc-cli serve openapi.yaml
   ```

3. Open [http://localhost:8080](http://localhost:8080) to view the docs

### Alternative: Docker

For a quick setup without installing tools:

```bash
# Swagger UI
docker run -p 8080:8080 -e SWAGGER_JSON=/foo/openapi.yaml -v $(pwd):/foo swaggerapi/swagger-ui

# Redoc
docker run -p 8080:80 -e SPEC_URL=openapi.yaml -v $(pwd):/usr/share/nginx/html/openapi.yaml redocly/redoc
```

## Troubleshooting

### Missing API Base URL Error

If you see:
```
Error: NEXT_PUBLIC_API_BASE_URL environment variable is required
```

**Solution:** Ensure `.env.local` exists in `apps/aira-web/` with the correct base URL.

### CORS Errors

If you encounter CORS issues:
- Verify the API base URL is correct
- Check that the backend allows requests from `localhost:3000`
- Ensure you're using the development API (`dev.api.airaai.in`) for local testing

### Authentication Issues

If authentication fails:
- Clear your browser cookies
- Verify the `NEXT_PUBLIC_GOOGLE_AUTH_URL` points to the correct auth endpoint
- Check browser console for token-related errors

## API Endpoints Overview

The API client in this repository makes calls to various endpoints:

- `/v1/users/me` - Get current user profile
- `/auth/oauth/link` - OAuth connection linking
- `/auth/oauth/exchange` - OAuth token exchange
- `/auth/google` - Google OAuth authentication
- And more (see `packages/core/src/api/` for all endpoints)

## Need Help?

- Check the main [README.md](./README.md) for general setup instructions
- Review `apps/aira-web/src/lib/api.ts` to see how the API client is configured
- Inspect `packages/core/src/api/` for shared API utilities and type definitions

## Security Notes

- Never commit `.env.local` to version control (it's in `.gitignore`)
- Do not expose sensitive API keys or tokens in `NEXT_PUBLIC_*` variables
- The `NEXT_PUBLIC_` prefix makes variables accessible in the browser, so only use it for non-sensitive configuration like base URLs
