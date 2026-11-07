# SEDUME AI PRO — Google Veo starter

A mobile-friendly web frontend plus a Node.js backend. The backend keeps the Google API key private and calls the Google Gemini API video-generation operation.

## Important limitations

- This app generates **one short Veo clip per request**. It does not create a single 10-minute video in one request.
- To make a 10-minute episode, generate multiple scenes and assemble them in a video editor. Automatic stitching, voice generation, image generation, captions and project persistence are not included in this starter.
- Google model availability, account eligibility, regional access, quotas and pricing can change. Check Google's current official documentation and your account's terms before enabling billing or making requests. Veo 3.1 has no free video-generation tier in the currently published Gemini API pricing.
- Do not share your API key. Do not put it in `public/app.js`, HTML, screenshots or a public repository.
- The included in-memory job store resets when the server restarts. For production, add a database and durable job queue.
- The `/api/jobs/:id/cancel` route stops local polling/status tracking; it does not guarantee cancellation of a provider-side Veo operation.

## Requirements

- Node.js 20+
- A Google Gemini API key from an account/project that is eligible to use Veo
- Network access from the server to Google's API

## Run locally

1. Extract the project.
2. Open a terminal in the project folder.
3. Run `npm install`.
4. Copy `.env.example` to `.env`.
5. Put your authorized key in `.env` as `GEMINI_API_KEY=...`.
6. Run `npm start`.
7. Open `http://localhost:3000` in a browser.

If you are not eligible to create or manage the API project, ask a parent/guardian or authorized project administrator to manage the account, billing and key. Do not falsify age or bypass account restrictions.

## Deployment

Deploy the Node app to a trusted Node.js host. Configure `GEMINI_API_KEY` as a server-side environment secret in the host dashboard. Do not upload `.env`. The default model is `veo-3.1-fast-generate-preview`. Set `VEO_MODEL` to another model only if your Google project has access and you understand its pricing. Use HTTPS, persistent storage, authentication, per-user quotas and billing limits before sharing the site publicly.

## Cost warning

Google's published pricing lists Veo 3.1 Fast at $0.10/second for 720p and $0.12/second for 1080p; Standard is $0.40/second for 720p/1080p. Pricing can change. A 10-minute finished video made from 8-second clips could require roughly 75 clips, so generation can become expensive. Have an authorized adult review pricing, billing and usage limits before enabling generation. Sources: https://ai.google.dev/gemini-api/docs/pricing

## API

- `GET /api/health` — backend/provider configuration status
- `POST /api/generate` — body: `{ "prompt": "...", "aspectRatio": "16:9" }`
- `GET /api/jobs/:id` — job status and app-relative video URL
- `GET /api/jobs/:id/video` — streams the generated video through the backend
- `POST /api/jobs/:id/cancel` — stops local tracking

## Troubleshooting

- `Veo not configured`: `.env` missing or `GEMINI_API_KEY` not set; restart server after changing it.
- `model not found` / `permission denied`: verify the current Veo model ID and access for your Google project.
- `quota` / `billing` errors: check your provider project quota and billing settings. Do not assume the API is free.
- Video download errors: check that the operation completed and that the API key still has permission to retrieve the generated file.
