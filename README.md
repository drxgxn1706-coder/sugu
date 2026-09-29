# AR Digital Creates — Vercel Edition

This ZIP is a Vercel-ready version of the AR Digital Creates website. It removes the Hatchable-specific backend and replaces it with Vercel Functions + PostgreSQL + Vercel Blob.

## What is included

- `index.html` — main AR Digital Creates landing page
- `styles.css` — cinematic dark/gold styling and animations
- `app.js` — services, portfolio, team, clients, enquiry form and frontend logic
- `admin/` — Studio Admin UI with login
- `api/` — Vercel serverless functions
- `api/_lib/` — database and admin-auth helpers
- `database/schema.sql` — PostgreSQL tables for enquiries and media
- `package.json` — Vercel dependencies
- `vercel.json` — Node.js 22 function configuration
- `.env.example` — environment variable template

## Deploy to GitHub + Vercel

### 1. GitHub

Extract this ZIP. Upload **all files and folders inside it** to the root of your GitHub repository.

Your repository should show files such as:

```text
index.html
app.js
styles.css
package.json
vercel.json
api/
admin/
database/
```

Do not upload only `README.md` and `hatchable.toml` from the old project.

### 2. Import into Vercel

Import the GitHub repository into Vercel.

Recommended settings for this plain HTML + Vercel Functions project:

- Framework Preset: Other / no framework
- Build Command: leave empty
- Output Directory: leave empty
- Install Command: `npm install`

Vercel automatically deploys files under `/api` as Node.js Functions.

### 3. Create the database

Create a PostgreSQL database. Neon is a convenient option for Vercel.

Copy the database connection string into Vercel:

`DATABASE_URL`

Then open the database SQL editor and run:

`database/schema.sql`

### 4. Create media storage

In the Vercel project, create a **Blob** store and connect it to this project.

The Blob store should be **Public** because portfolio/team/hero media is intended to be displayed on the public website.

Vercel will provide the Blob environment variable needed by the SDK.

### 5. Configure Studio Admin

Add these Vercel Environment Variables:

`ADMIN_USERNAME=your-admin-name`

`ADMIN_PASSWORD=your-strong-password`

Use the same values in Production (and Preview if you want the admin available there).

### 6. Redeploy

After adding the database, Blob store and admin environment variables, redeploy the project.

Website:

`https://YOUR-PROJECT.vercel.app/`

Studio Admin:

`https://YOUR-PROJECT.vercel.app/admin/`

## Important upload note

The included admin upload route uses a server upload and intentionally limits a single upload to 4 MB because Vercel Functions have a 4.5 MB request-body limit. For large videos/showreels, use Vercel Blob client uploads/direct uploads instead of sending the file through the Function.

## Security

- Database credentials are server-side environment variables.
- Admin endpoints require HTTP Basic authentication using the Vercel environment variables.
- Do not put `DATABASE_URL`, admin passwords or Blob credentials in `app.js`, `index.html` or other browser code.
- The admin credential is kept only in the browser tab's session storage while the admin page is open.

## Current site content

The visual design is based on the existing AR Digital Creates v4 site, including formal Manrope typography, cinematic dark/gold styling, services, team, clients, portfolio, showreel/BTS sections and enquiry form.

## Current architecture

```text
Browser
  │
  ├── Static HTML/CSS/JS on Vercel
  │
  ├── /api/enquiries ──────────> PostgreSQL
  ├── /api/assets ─────────────> PostgreSQL
  │
  └── /admin
       ├── /api/admin/session ─> Admin auth
       ├── /api/admin/enquiries > PostgreSQL
       ├── /api/admin/assets-list > PostgreSQL
       └── /api/admin/assets ──> Vercel Blob + PostgreSQL
```
