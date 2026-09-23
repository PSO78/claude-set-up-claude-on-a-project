# CLAUDE.md

Starter Express API for the Claude Code course projects.

## Commands
- `npm run dev` — start the API with auto-reload on http://localhost:3000
- `npm test` — run the test suite (Node's built-in test runner)
- `npm run lint` — check code style with ESLint

Run a single test file: `node --test tests/users.test.js`

## Architecture

- `server.js` — creates the Express app, mounts one router per resource, and exports `app` without calling `listen()` when required as a module (`require.main === module` guard). Tests import `app` directly via `supertest`, so no real port is opened during `npm test`.
- `routes/` — one file per resource (`users.js`, `health.js`), each exporting an `express.Router()`. Route handlers call into `db/store.js` rather than touching data directly.
- `db/store.js` — in-memory data layer standing in for a real database. State (`users`, `nextId`) resets on every server restart; there is no persistence.
- `tests/` — uses `node:test` + `node:assert` + `supertest` against the exported `app`, not a running server.

## Conventions

- Use CommonJS (`require` / `module.exports`), not ES module `import`.
- Data access from routes goes through `db/store.js`, not inline array manipulation.
- One route file per resource in `routes/`, mounted in `server.js`.
- Validate input in the route: return `400` for bad input and `404` for a missing record.
- `.env` holds real config/secrets.