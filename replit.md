# Unhinged Economics Online

A multiplayer policy game where every room starts with the same absurd economic crisis, then each chapter is driven by one player's answer.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 5000)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `artifacts/unhinged-economics/src/App.tsx` — responsive lobby, room, chapter, timer, metrics, and results UI.
- `artifacts/api-server/src/game.ts` — in-memory Socket.IO rooms, 20-chapter story arc, unique answer deck, and answer-driven progression.
- `artifacts/api-server/.replit-artifact/artifact.toml` — API and websocket proxy paths.

## Architecture decisions

- Game state is intentionally in memory; a room is a live briefing, not a saved account or campaign.
- Chapter one is shared, then the story driver rotates through the room so every player can steer a later consequence.
- Each round's answer cards include the chapter identity, which guarantees no visible answer repeats within a match.
- The API service owns Socket.IO so the Vite frontend stays a static deployable artifact.

## Product

- Create or join a room with 10, 15, or 20 chapters.
- Reconnect within the current browser session.
- Make private policy choices while seeing live delegate status, timers, economy indicators, and final rankings.

## User preferences

No additional preferences recorded.

## Gotchas

- The Socket.IO path is `/api/socket.io`; it must stay listed in the API artifact paths or browser connections will silently fail.
- The API service must be restarted after room-server changes so the managed build picks up the new bundle.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
