# Aira - Frontend Assignment

Aira is your AI assistant that works 24/7 on your behalf. The main interaction happens through WhatsApp — you chat with Aira directly, and it manages your life across groups, email, calendar, and more.

The core concept is **rules**: you tell Aira what to watch for ("notify me if someone mentions my name", "flag anything marked urgent", "summarize this group every morning at 9am"), and Aira follows them automatically.

## What's Inside

This is a Turborepo monorepo with the following structure:

### Apps

- **`apps/aira-web`** — Next.js web dashboard ([app.airaai.in](https://app.airaai.in)) where users configure connectors, rules, and manage tasks

### Packages

- **`packages/core`** — Shared API client, schemas, auth utilities, and stores used by both apps
- **`packages/typescript-config`** — Shared `tsconfig.json` configurations

### Tech Stack

- **TypeScript** everywhere
- **Next.js** (web)
- **TanStack Query** for data fetching
- **Zustand** for state management
- **Zod** for schema validation
- **Axios** for HTTP

**📖 Need help configuring the API?** See [API_CONFIGURATION.md](./API_CONFIGURATION.md) for detailed setup instructions.

## Getting Started

### Prerequisites

- Node.js >= 18
- pnpm (recommended) or npm

### Setup

```bash
# Install dependencies
pnpm install

# Configure API (see API_CONFIGURATION.md for details)
cd apps/aira-web
cp .env.example .env.local
# Edit .env.local to set NEXT_PUBLIC_API_BASE_URL=https://dev.api.airaai.in

# Start the web app in development
cd ../..
pnpm dev --filter=aira-web


## The Assignment

**Deadline:** 3 days from receiving the assignment email

### What to do

1. Go to [airaai.in](https://airaai.in) → click "Try Aira" to get started
2. Connect your accounts, set up some rules, feel the friction
3. Find something that bugs you — onboarding, navigation, a specific flow, whatever
4. Build a working improvement

### What we're looking for

- Good taste
- Clear thinking about UX problems
- Code that works

### How to submit

Reply to the assignment email with your code and a short note explaining what you fixed and why.

## Project Structure

```
├── apps/
│   ├── aira-web/          # Next.js dashboard
│   │   ├── src/
│   │   │   ├── app/       # Next.js app router pages
│   │   │   ├── components/
│   │   │   └── lib/       # API client, utilities
├── packages/
│   ├── core/              # Shared business logic
│   │   └── src/
│   │       ├── api/       # API client & endpoints
│   │       ├── auth/      # Auth utilities
│   │       ├── schemas/   # Zod schemas
│   │       └── stores/    # Zustand stores
│   └── typescript-config/ # Shared TS configs
└── turbo.json
```

## License

MIT — see [LICENSE](./LICENSE)
