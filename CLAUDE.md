# CLAUDE.md

Starter Express API for the Claude Code course projects.

## Commands

npm install
npm run dev      # starts the API on http://localhost:3000, auto-restarts on change (node--watch)
npm test         # runs all tests (node's built-in test runner)

Run a single test file: `node --test tests/users.test.js`

CI (`.github/workflows/ci.yml`) runs `npm install`, `npm run lint`, `npm test` on every push and PR — keep both green.

## Architecture

- `server.js` — creates the Express app, mounts one router per resource, and exports `app` without calling `listen()` when required as a module (`require.main === module` guard). Tests import `app` directly via `supertest`, so no real port is opened during `npm test`.
- `routes/` — one file per resource (`users.js`, `health.js`), each exporting an `express.Router()`. Route handlers call into `db/store.js` rather than touching data directly.
- `db/store.js` — in-memory data layer standing in for a real database. State (`users`, `nextId`) resets on every server restart; there is no persistence.
- `tests/` — uses `node:test` + `node:assert` + `supertest` against the exported `app`, not a running server.

## Conventions

- Data access from routes goes through `db/store.js`, not inline array manipulation.
- `.env` holds real config/secrets.