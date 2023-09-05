# Frontend Setup using Nuxt 3

## Requirements

Install Node.js version v16.18+

## Setup

Make sure to install the dependencies:

```bash
# npm
npm install

# pnpm
pnpm install

# yarn
yarn install
```

## Environment Setup

Create an `.env` file in the root directory of your project with the API Keys from Supabase.

You can find your API keys [here](https://supabase.com/dashboard/project/_/settings/api).

```
SUPABASE_URL=https://`<project>`.supabase.co
SUPABASE_KEY=<your-anon-key>
```

⚠️  **Security Note** : Never commit your `.env` file or any sensitive keys to public repositories!

## Development Server

Start the development server on `http://localhost:3000`:

```bash
# npm
npm run dev

# pnpm
pnpm run dev

# yarn
yarn dev
```

## Production

Build the application for production:

```bash
# npm
npm run build

# pnpm
pnpm run build

# yarn
yarn build
```

Locally preview production build:

```bash
# npm
npm run preview

# pnpm
pnpm run preview

# yarn
yarn preview
```

Check out the [deployment documentation](https://nuxt.com/docs/getting-started/deployment) for more information.
