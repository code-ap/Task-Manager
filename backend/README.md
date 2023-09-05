# Backend Setup Using Supabase

Supabase is utulized as Backend-as-a-Service (BaaS). There are two primary methods to get started with Supabase for this project:

## Option 1: Using Supabase.com

1. Go to [Supabase Dashboard](https://supabase.com/dashboard/).
2. If you're new, sign up for an account or log in using GitHub.
3. Once logged in, click on "Create a new project."
4. Import the configurations using the code provided in this repository.

## Option 2: Self-hosting Supabase

1. Install `git` and `Docker`
2. Follow the official Guide [Self-Hosting with Docker](https://supabase.com/docs/guides/self-hosting/docker).
3. Once your local Supabase instance is up and running, import the configurations using the code provided in this repository.

# Setup

Create new table profile for additional user data

```
`
create table public.profiles (
  id uuid not null references auth.users on delete cascade,
  first_name text,
  last_name text,
  primary key (id)
);
```

Policy for all users to be able to view profiles table

```
`
CREATE POLICY "Enable read access for authenticated users" ON "public"."profiles"
AS PERMISSIVE FOR SELECT
TO public
USING ((auth.uid() IS NOT NULL))
```

Handling User Sign-ups to automatically also create a profiles table

```
`
-- Function to handle new user sign-ups
create function public.handle_new_user()
returns trigger
language plpgsql
security definer set search_path = public
as $$
begin
  insert into public.profiles (id)
  values (new.id);
  return new;
end;
$$;

-- Trigger the function every time a user is created
create trigger on_auth_user_created
  after insert on auth.users
  for each row execute procedure public.handle_new_user();

```

Create tasks table with reference to profiles table

```
`
create table tasks(
   id serial primary key,
   title varchar(255) not null,
   status varchar(50) not null default 'backlog',
   deadlineDate date,
   deadlineTime time,
   description text
   creator uuid references profiles (id), -- referencing profiles table
   responsible uuid references profiles (id),
);
```