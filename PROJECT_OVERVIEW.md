# Project Overview: Green Madurai

**Green Madurai** is an educational React/Vite application designed to gamify waste segregation for school children in the Madurai region. The app leverages AI to analyse waste images and transform everyday recycling into a fun, interactive experience.

## Key Features

- **Image-based Waste Detection**: Children take a photo of waste using their device camera. The image is sent to the backend for AI analysis.
- **Gemini AI Integration**: Uses Google Gemini Vision API via `@google/genai` to classify waste items and produce a structured JSON response.
- **Eco‑Assist Mode**: A friendly teacher-like AI responds with categorization (`biodegradable`, `plastic`, `mixed`, `unknown`), tips, `didYouKnow` facts, next actions, encouragements, and bonus points. No shaming.
- **Knowledge Base**: Lightweight expandable datastore (`src/services/ecoKnowledgeBase.ts`) contains information for common waste items (decomposition times, tips, etc.).
- **Gamification**: Points awarded for correct segregation, with confetti and badges. Extra points are granted when children read the tip.
- **Leaderboard & Class Progress**: Backend tracks scores; front end displays leaderboard and progress toward collective rewards.
- **Responsive UI with Motion**: Built using React, Tailwind CSS, and Motion for animations. Supports mobile capture.
- **Privacy & Safety**: Images are analysed transiently; no face data or metadata stored. Images are deleted post-analysis.

## Technical Structure

- **Backend (`server.ts`)**
  - Express server with routes `/api/leaderboard`, `/api/score`, and `/api/eco-analyze`.
  - Development uses Vite middleware; production serves `dist`.

- **API Services (`src/services`)
  - `api.ts` exposes helper functions to call backend and Gemini.
  - `aiEducationEngine.ts` orchestrates AI request and knowledge-base lookup.
  - `ecoKnowledgeBase.ts` holds static educational data.

- **Controllers (`src/controllers`)
  - `ecoAssistController.ts` handles image analysis endpoint.

- **Frontend pages**
  - `Home.tsx`: main interaction page for capturing images, showing results, and handling scores.
  - Additional pages (Leaderboard, Login, etc.) exist for navigation and account.

## Development

1. Clone repo and `cd build-for-madurai`.
2. `npm install` to fetch dependencies.
3. Set environment variables in `.env.local` (e.g. `GEMINI_API_KEY`, `APP_URL`).
4. Run `npm run dev` to start the dev server on port 3000.

## Deployment

- Build with `npm run build`.
- Deploy as a Node server (e.g. Cloud Run) using `npm start`.
- Ensure environment variables are provided by the host.

### Production: pointing frontend to a separate API host

If you serve the frontend assets from a static host or CDN and the API runs on a different host, set `VITE_APP_URL` at build time so the browser calls the correct API origin. `VITE_APP_URL` is embedded into the bundle at build time (Vite requires the `VITE_` prefix); `APP_URL` remains a runtime variable used by the server.

- Example (Linux/macOS):
```bash
VITE_APP_URL=https://api.example.com npm run build
```
- Example (PowerShell / Windows):
```powershell
$env:VITE_APP_URL = 'https://api.example.com'
npm run build
```

After building, deploy the `dist` output to your static host. Ensure the API is reachable at `https://api.example.com/api/*` (or the host you configured). If your static host and API are on the same domain, you can leave `VITE_APP_URL` blank to use relative `/api` paths.

## Future Enhancements

- Toggle Tamil-language tips and voice-based explanations.
- Expand the knowledge base with regional waste examples.
- Add user accounts, class management, and persistent leaderboard.
- Offline first support and camera improvements.

---

This overview serves as a quick reference for developers onboarding onto Green Madurai. Feel free to update it as the project evolves.