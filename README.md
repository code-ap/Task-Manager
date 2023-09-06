# TaskManager with Nuxt 3 and Supabase

**TaskManager** is a task management application using the power of Nuxt 3 for the frontend and Supabase as a Backend-as-a-Service (BaaS). It aims to provide a seamless experience for users to manage and track tasks with collaboration features. Currently, the application is in its early stages, with primary functionalities focused on task management.

## Features

* **Task Management** : Create, update, delete, and view tasks.
* **Collaboration** : Groups feature for users to collaborate on tasks (coming soon).

## Frontend Setup using Nuxt 3

### Requirements

Install Node.js version v16.18+

### Setup

Make sure to install the dependencies:

```bash
# npm
npm install

# pnpm
pnpm install

# yarn
yarn install
```

### Environment Setup

Create an `.env` file in the root directory of your project with the API Keys from Supabase.

You can find your API keys [here](https://supabase.com/dashboard/project/_/settings/api).

```
SUPABASE_URL=https://`<project>`.supabase.co
SUPABASE_KEY=<your-anon-key>
```

⚠️  **Security Note** : Never commit your `.env` file or any sensitive keys to public repositories!

### Development Server

Start the development server on `http://localhost:3000`:

```bash
# npm
npm run dev

# pnpm
pnpm run dev

# yarn
yarn dev
```

### Production

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

## Backend Setup Using Supabase

Supabase is utilized as Backend-as-a-Service (BaaS). There are two primary methods to get started with Supabase for this project:

### Option 1: Using Supabase.com

1. Go to [Supabase Dashboard](https://supabase.com/dashboard/).
2. If you're new, sign up for an account or log in using GitHub.
3. Once logged in, click on "Create a new project."
4. Import the configurations using the code provided in this repository.

### Option 2: Self-hosting Supabase

1. Install `git` and `Docker`
2. Follow the official Guide [Self-Hosting with Docker](https://supabase.com/docs/guides/self-hosting/docker).
3. Once your local Supabase instance is up and running, import the configurations using the code provided in this repository.

### Database Setup

Set up the necessary tables, triggers, and policies using the SQL commands from README in backend.

## Future Enhancements

* **Groups Feature** : Allow users to create groups and collaborate on tasks within these groups.
