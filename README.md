# Bidur. Portfolio + CMS

A dark editorial portfolio for a video editor and motion designer. Built with React, Vite, Cloudflare Pages Functions, and D1. Categories and projects are database-driven; there are no hardcoded portfolio categories.

## Included
- Responsive portfolio with Syne, Outfit, and Playfair Display
- Category filters populated from D1
- YouTube embeds and automatic thumbnail URLs
- Admin login, category CRUD, project CRUD, draft/publish and featured controls
- API-side password check and signed HttpOnly session cookie
- Cloudflare Pages deployment config and D1 schema

## Deploy on Cloudflare Pages for $0

You need a Cloudflare account and a GitHub account. Free plans have usage limits and terms can change.

1. In Cloudflare Dashboard, go to **Workers & Pages → D1 SQL Database** and create a database named `bidur-portfolio-db`.
2. Open the database's Console and run the contents of `schema.sql`.
3. Copy the database ID into `wrangler.toml`, replacing `REPLACE_WITH_YOUR_D1_DATABASE_ID`. This must be done before the first deployment.
4. Create a GitHub repository and push this project.
5. In Cloudflare Dashboard, go to **Workers & Pages → Create → Pages → Connect to Git** and select the repository.
6. Build command: `npm run build`; build output directory: `dist`.
7. Confirm the D1 binding is named `DB` and points to `bidur-portfolio-db` in the Pages/Functions configuration.
8. In **Settings → Variables and Secrets**, add:
   - `ADMIN_PASSWORD`: choose a long, unique password.
   - `SESSION_SECRET`: a separate random secret, at least 32 random characters. Do not reuse your admin password.
9. Deploy (or redeploy) after confirming the binding and secrets.
10. Open your deployed URL and select **Studio Login** in the footer.

For a local secret you can generate one with `openssl rand -hex 32`. Keep secrets only in Cloudflare environment settings, never in frontend code or Git.

## Local development

```bash
npm install
npm run dev
```

The UI can run under Vite locally, but the `/api` backend requires the Cloudflare Pages runtime and D1 binding. For a local end-to-end test, install Wrangler, configure `.dev.vars` with `ADMIN_PASSWORD=...` and `SESSION_SECRET=...`, create a local D1 database, and use `wrangler pages dev` after building. Do not commit `.dev.vars`.

## Notes
- Videos are embedded from YouTube, not uploaded to this site. Only use videos you have permission to display.
- The admin uses a signed, HttpOnly, Secure, SameSite=Strict cookie with a seven-day expiry. Use HTTPS (Cloudflare Pages provides it).
- Protect access to your Cloudflare account, keep your database schema/backups safe, and rotate secrets if exposed.
- This is a starter implementation. Before high-stakes production use, add rate limiting to login and a scheduled/verified backup process for D1.
- The email address in the contact section is a placeholder (`hello@bidurp.com`). Replace it in `src/main.tsx` with the address you actually want to publish.
