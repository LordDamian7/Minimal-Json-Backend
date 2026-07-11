# Minimal JSON Backend

The smallest possible backend: a single Node.js file, zero dependencies, two JSON endpoints. CORS is enabled so it works from `curl` and directly from browser JavaScript (`fetch`).

## Run

```bash
node server.js
```

Server starts on `http://localhost:4000` (override with `PORT=5000 node server.js`).

## Endpoints

### `GET /api/health`

```bash
curl http://localhost:4000/api/health
```

```json
{ "status": "ok", "uptimeSeconds": 12.3, "timestamp": "2026-07-11T00:00:00.000Z" }
```

Also visitable directly in a browser at `http://localhost:4000/api/health`.

### `POST /api/echo`

```bash
curl -X POST http://localhost:4000/api/echo \
  -H "Content-Type: application/json" \
  -d '{"hello":"world"}'
```

```json
{ "received": { "hello": "world" }, "timestamp": "2026-07-11T00:00:00.000Z" }
```

From a browser console:

```js
fetch("http://localhost:4000/api/echo", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ hello: "world" }),
}).then((r) => r.json()).then(console.log);
```

## Deploying on Vercel

`server.js` is a plain long-running Node server, meant for local use — Vercel doesn't run
those. Instead, the same two endpoints are also implemented as Vercel serverless functions
in `api/health.js` and `api/echo.js`, which Vercel auto-detects with no config needed.

To deploy: on vercel.com, **Add New → Project → Import** this GitHub repo, leave all
settings at their defaults, and deploy. Once live, the endpoints are available at:

- `https://<your-project>.vercel.app/api/health`
- `https://<your-project>.vercel.app/api/echo`

Both work the same way as the local versions above — just swap `http://localhost:4000` for
your Vercel deployment URL.
