# Student Project Tracker (Cloud Variant)

A cloud-ready variant of the hands-on web application development workshop project, adapted for hosting on **Vercel** with a managed **Supabase PostgreSQL** database.

## Architecture

```text
Browser (UI) <---> fetch (/api/projects) <---> Express (server.js / project-api.js) <---> PostgreSQL (database.js)
[HTML / CSS / JS]         [JSON REST API]                 [Hosted on Vercel]                [Supabase Cloud DB]
```

- **Frontend**: Preserves the original semantic HTML5, CSS3, and JavaScript in `public/`.
- **API Layer**: `project-api.js` defines the route handlers and validation; `server.js` exports the configured Express application for Vercel serverless deployment.
- **Database**: `database.js` connects to PostgreSQL using `postgres` (with connection pooling and SSL), reading the server-only `DATABASE_URL` environment variable.
- **Schema**: Defined in `schema.sql` (`public.projects` with identity primary key, validation constraints, and Row Level Security enabled).

## Local Rehearsal

To run and test the cloud variant locally against the managed PostgreSQL database:

1. **Install dependencies**:
   ```bash
   npm ci
   ```

2. **Configure environment**:
   - Copy `.env.example` to `.env`:
     ```bash
     cp .env.example .env
     ```
   - Set your private connection string in `.env`:
     ```text
     DATABASE_URL=postgresql://postgres:[YOUR-PASSWORD]@[HOST]:[PORT]/postgres?sslmode=require
     ```
   *(Never commit `.env` to source control. It is excluded by `.gitignore`.)*

3. **Database initialization**:
   - Run the SQL script `schema.sql` in the Supabase SQL Editor to create the `projects` table.

4. **Start the local server**:
   ```bash
   npm start
   ```
   Open [http://localhost:3000](http://localhost:3000) (runs `node --env-file=.env live-local.js`).

## Deployment (Vercel)

1. Push this repository to GitHub (ensure `.env` is **not** pushed).
2. Import the repository into Vercel.
3. In the Vercel project settings, configure the environment variable:
   - `DATABASE_URL`: *(Your Supabase connection string)*
4. Deploy the project. The hosted URL will be added here once deployed.

- **Hosted URL**: *(Pending deployment)*

## Verified Checks

- Verified `/api/health` returning `{"ok": true}`.
- Verified local live rehearsal connected to Supabase PostgreSQL:
  - Table creation via `schema.sql` in Supabase SQL editor.
  - Adding records (`Cloud Test` / `Planned`) and confirming rows persist in the Supabase Table Editor.
  - Updating status to `Completed`.
  - Deleting disposable records with confirmation prompt.
  - Server restart persistence test against the cloud database.

## Limitations

- This is a shared teaching demonstration without user authentication or login isolation; all users share the project collection.
- Local SQLite rows from previous workshops are not automatically migrated.
- `DATABASE_URL` is private and must be configured per deployment environment.
