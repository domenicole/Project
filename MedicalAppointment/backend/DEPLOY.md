# Deployment notes for Render / Vercel

This file explains environment variables and CORS settings used by the backend and recommended deployment steps.

## Important environment variables

- ALLOWED_ORIGINS (optional): comma-separated list of allowed origins for CORS. If not set, the backend uses a safe default list and also allows `*.vercel.app` previews.
  - Example: `ALLOWED_ORIGINS=https://t6-medical-appointment.vercel.app,https://t6-medical-appointment-bropphl4c-domenicas-projects-58f1b051.vercel.app`

- FRONTEND_URL (optional): used by OAuth redirect flows to build correct redirect URLs.
  - Example: `FRONTEND_URL=https://t6-medical-appointment.vercel.app`

- JWT_SECRET: same as before, required to sign tokens.

## Recommendations for Render (backend)

1. Commit and push the changes to your repo and trigger a deploy on Render.
2. If you want to control allowed origins precisely, add `ALLOWED_ORIGINS` in the Render environment settings and include your production Vercel domains.
3. If you rely on credentials/cookies, ensure `ALLOWED_ORIGINS` lists the exact origins (using `*` is not valid when `credentials: true`).
4. Use the debug endpoint `GET /api/debug/cors` (available in the server) after deploy to confirm the server is receiving origins correctly.

## Recommendations for Vercel (frontend)

1. Keep **Build Command** as `npm run build` (or `yarn build`) and **Output Directory** as `dist`.
2. The `build` script has been changed to run Vite via Node to avoid `Permission denied` errors in CI.
3. Make sure the Vercel project uses a compatible Node version (if needed, set `engines` in `package.json` or set the Node version in Vercel settings).

## Quick verification

- Preflight test (from your terminal):

  `curl -i -X OPTIONS "https://<your-backend>/api/auth/login" -H "Origin: https://<your-frontend>" -H "Access-Control-Request-Method: POST"`

- Debug endpoint (after deploy):

  `curl -i -H "Origin: https://<your-frontend>" "https://<your-backend>/api/debug/cors"`

If you want, I can also prepare a small script or Git commit message for you to push these changes; tell me if you want that next.