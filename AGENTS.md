# Potato OS Apps Repository

This repo contains optional, process-isolated apps for Potato OS. The built-in Chat app stays in [`potato-os/core`](https://github.com/potato-os/core).

## App Structure

Each app lives in `apps/<id>/` and follows the platform contract:

| File | Required | Purpose |
|------|----------|---------|
| `app.json` | yes | Manifest — identity, process config, lifecycle |
| `main.py` | yes | Entry point (runs as subprocess) |
| `routes.py` | no | FastAPI router, mounted at `/app/<id>/api/` |
| `lifecycle.py` | no | `on_startup` / `on_shutdown` hooks |
| `rig.md` | no | RIG workflow contract (steps, flow graph, schemas) |
| `install.sh` | no | Device install hook (called by `install_dev.sh`) |
| `assets/` | no | Static UI files, served at `/app/<id>/assets/` |

### Import Convention

Apps use `from apps.<id>.module import thing`. This works because:
- On device: `app_routes.py` adds `/opt/potato` to `sys.path`
- In tests: `pytest.ini` sets `testpaths = apps`

## Deployment

Apps from this repo are installed to a Potato device via the `POTATO_APPS_REPO` env var:

```bash
# Deploy chat (built-in) + permitato (from this repo)
POTATO_APPS_REPO=/path/to/this/repo \
  POTATO_IMAGE_APPS=chat,permitato \
  ./bin/install_dev.sh
```

The platform discovers apps by scanning `/opt/potato/apps/*/app.json` — no platform code changes needed.

## Running Tests

### Python unit tests

```bash
python -m pytest apps/ -q
```

### Playwright tests

Playwright tests require a running Potato platform at `http://127.0.0.1:1983`:

```bash
npx playwright test
```

## Apps

- **permitato** — Pi-hole-backed attention guard app
- **skeleton** — Template app for building new Potato apps

## Coding Style

Follow the same conventions as `potato-os/core`:
- Python 3.11+, 4-space indent, type hints for new/changed code
- `snake_case` for functions/files, `PascalCase` for classes
- Frontend: vanilla JS, no frameworks, no build step
- Shell scripts: `set -euo pipefail`
