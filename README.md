# Apeiro

Apeiro is a small AI-powered choose-your-own-adventure web game.

- You enter a starting prompt (theme/setting).
- The server uses Gemini to generate a scene + 4 choices.
- Each choice advances the story, tracks health, and updates the background image.

The app is an Express server that renders EJS pages and exposes a small JSON API (documented via Swagger UI).

## Demo

[![Apeiro Demo](https://img.youtube.com/vi/y2gxm0guUM4/0.jpg)](https://www.youtube.com/watch?v=y2gxm0guUM4)

## Tech

- Node.js + Express + EJS
- Google Gemini (`@google/generative-ai`) for story + image prompts
- Pollinations (`image.pollinations.ai`) for free scene images (no key)
   - NOTE: Unfortunately, the free Pollinations API has been deprecated.
- OpenAPI validation + Swagger UI at `/v0/api-docs`

## Setup (minimal)

The only required “external” setup is a Gemini API key.

1) Create `apeiro/app/.env` with:

```bash
GOOGLE_API_KEY=your_gemini_api_key
```

Then install + run:

```bash
cd apeiro/app
npm install
npm start
```

Open:

- http://localhost:3010/ (UI)
- http://localhost:3010/v0/api-docs/ (API docs)

### Optional env vars

- `PORT` (default: `3010`)

## How to play

1) Go to `/explore`
2) Enter a prompt and click **Start**
3) Click an option (or use the custom prompt button) to continue

## API overview

Key endpoints (see Swagger UI for the full schema):

- `POST /v0/create-story` → start a story from `{ "prompt": "..." }`
- `POST /v0/choose-decision` → advance with `{ "number": 1..4, "custom"?: "..." }`
- `GET /v0/current-scene` → current scene object
- `GET /v0/current-decisions` → current list of decisions
- `GET /v0/scenes` → all scenes so far

## Project layout

- `apeiro/app/src/server.js`: server entrypoint
- `apeiro/app/src/app.js`: Express app wiring + routes + OpenAPI middleware
- `apeiro/app/src/story.js`: route handlers (delegates to Gemini client)
- `apeiro/app/src/geminiClient.js`: story generation + health/round logic
- `apeiro/app/src/imageGenerator.js`: downloads scene images into `static/css/images/scenes/`
- `apeiro/app/src/views/`: EJS pages
- `apeiro/app/src/static/`: CSS/JS/assets

## Notes

- Generated scene images are written under `apeiro/app/src/static/css/images/scenes/` and are ignored by git.
- The game requires outbound network access (Gemini + Pollinations).

