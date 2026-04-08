# Potato OS Apps

Process-isolated apps for Potato OS. These are optional apps that can be installed alongside the built-in Chat app.

## Apps

- **permitato** — Pi-hole-backed attention guard
- **skeleton** — Template app for building new Potato apps

## Development

Tests require the dependencies from the core platform. Install dev dependencies:

```bash
pip install -r requirements-dev.txt
```

Run Python tests:

```bash
python -m pytest apps/ -q
```

Playwright tests require a running Potato platform at `http://127.0.0.1:1983`:

```bash
npx playwright test
```

## Deployment

Apps from this repo are installed to a Potato device via the `POTATO_APPS_REPO` environment variable:

```bash
POTATO_APPS_REPO=/path/to/this/repo POTATO_IMAGE_APPS=chat,permitato ./bin/install_dev.sh
```

See the [core repo](https://github.com/potato-os/core) for full platform documentation.
