# Copilot / AI agent instructions for Heydrop

## Quick summary
- This is a Next.js (app router) demo app: UI in `app/` + components in `components/` + API routes under `app/api/*`.
- Storage is abstracted in `lib/storage.ts`: it uses Upstash Redis when configured, otherwise falls back to `lib/memory-store.ts`.
- Message/room TTLs are short-lived (24 hours). File uploads are handled locally in `app/api/uploadthing/route.ts` and return base64 data URLs (demo only).

## Local dev & common commands ✅
- Install: repo contains `pnpm-lock.yaml` → prefer `pnpm install`. `npm install` may fail if lockfile and package manager mismatch.
- Start dev server: `pnpm dev` (or `npm run dev`).
- Build / preview: `pnpm build` / `pnpm start` (or `npm run build` / `npm run start`).
- Lint: `pnpm lint` (uses `next lint`).

## Where to look (key files & what they show) 🔎
- `app/api/rooms/route.ts` — room creation (+ verification) and listing. Example: POST /api/rooms returns `{ roomId }`.
- `app/api/rooms/[roomId]/messages/route.ts` — messages GET/POST. POST expects JSON `{ content, type }` and returns the stored message.
- `app/api/uploadthing/route.ts` — small demo upload handler. Expects FormData `file` and returns `{ url, fileName, fileSize, fileType }` (data URL in local dev).
- `lib/storage.ts` — storage manager: uses env vars to decide Redis vs memory; read for error handling & fallback behavior.
- `lib/redis.ts` — Redis client init; note subtle inconsistency in env var names (see Environment Variables section).
- `lib/memory-store.ts` — fallback in-memory store (non-persistent, useful for local dev/tests).
- `store/roomStore.ts` — client-side Zustand store used by components for messages.
- `components/ContentInput.tsx`, `components/FileUpload.tsx` — how the UI uploads files and posts messages to the APIs.

## Environment variables & common pitfalls ⚠️
- Redis configuration (Upstash) is optional. The codebase checks for these names in different places:
  - `lib/storage.ts` checks: `UPSTASH_REDIS_REST_URL` & `UPSTASH_REDIS_REST_TOKEN`.
  - `lib/redis.ts` reads: `KV_REST_API_URL` & `KV_REST_API_TOKEN` and logs messages referencing `UPSTASH_*` names.
- Recommendation for agents: When verifying/setting env vars, check both sets (`UPSTASH_*` and `KV_*`). Use `/api/debug` and `/api/status` to confirm which env vars are present and whether Redis is active.
- Important endpoints for debugging:
  - GET `/api/debug` — returns presence of env vars and lists keys containing REDIS/KV/UPSTASH.
  - GET `/api/status` — returns `storageType` (Redis or Memory) and active rooms count.
  - POST `/api/cleanup` — triggers `storage.clearCorruptedData()` to remove corrupted Redis keys.

## Developer patterns & conventions 🧭
- Project uses TypeScript, `@/*` path alias (see `tsconfig.json`). Keep types in `lib/types.ts`.
- Client vs server boundaries: many components are client components (look for `"use client"` at top). Server code lives in `app/api/*` and server components.
- Use the existing API style (Next `route.ts` functions + `NextResponse.json`) and logging patterns (the code logs descriptive emoji-prefixed messages; follow this style when adding logs).
- Storage fallback: write code assuming either Redis or Memory may be active. Use `storage.*` methods rather than directly calling Redis where possible.
- Room IDs are short (generated in `lib/utils.ts` via `generateRoomId()`); messages and rooms expire after ~24 hours (see `ROOM_TTL` / `MESSAGE_TTL` constants).

## Small examples (how to perform common tasks) 💡
- Create a room (JS example):
  - POST `/api/rooms` -> returns `{ roomId }`.
- Post a message (JS example):
  - POST `/api/rooms/{roomId}/messages` with JSON `{ content: 'hi', type: 'text' }`.
- Upload a file (UI example): `ContentInput` sends FormData `file` to `/api/uploadthing`. Returned `url` is a data URL in this demo.

## Extra notes for agents / PR reviewers 🔧
- There is no test runner configured (no `test` script in `package.json`). Be conservative when changing storage behavior — add minimal manual tests or use `app/api/status` to validate changes.
- `next.config.js` disables PWA in dev and sets `typescript.ignoreBuildErrors = true`. Be careful: type errors may not fail CI builds by default.
- If adding persistent uploads, replace `app/api/uploadthing/route.ts` implementation and be mindful of file size limits (current demo caps at ~50MB).

---
If anything here is unclear or you want additional examples (e.g., a PR checklist, or a short code scaffold to add a new API route), tell me what to add and I'll iterate. 

Relevant files referenced in this doc:
`app/api/*`, `lib/storage.ts`, `lib/redis.ts`, `lib/memory-store.ts`, `components/ContentInput.tsx`, `components/FileUpload.tsx`, `store/roomStore.ts`, `lib/utils.ts`, `next.config.js`.