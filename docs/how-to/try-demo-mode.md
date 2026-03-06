# How to Try Stoneforge with Demo Mode

Demo mode lets you try Stoneforge without any API keys or paid subscriptions. It configures all agents to use the **opencode** provider with the **minimax-m2.5-free** model — a free-tier model that requires no authentication.

## Quick Start with Demo Mode

### 1. Install and initialize

```bash
npm install -g @stoneforge/smithy
cd your-project
sf init --demo
```

The `--demo` flag creates a standard Stoneforge workspace and configures all default agents (director, workers, steward) to use `opencode/minimax-m2.5-free`.

### 2. Start the server

```bash
sf serve
```

The dashboard opens at http://localhost:3457. From here you can manage tasks, watch agents, and control orchestration — all using the free model.

### 3. Follow the normal workflow

Register agents, start the Director, and give it a goal. See the Quick Start guide for the full workflow. Everything works the same as with a paid provider.

## What Demo Mode Does

When enabled, demo mode:

- Sets all agents to use provider: `opencode`, model: `minimax-m2.5-free`
- Persists `demo_mode: true` in `.stoneforge/config.yaml`
- Saves each agent's previous provider/model settings for restoration
- Requires no API keys — the model is free

All other Stoneforge features work normally: task planning, dependency tracking, worktree isolation, auto-dispatch, and merge stewards.

## Enabling and Disabling

### At initialization (CLI)

```bash
sf init --demo
```

### Via the dashboard

1. Open the dashboard at http://localhost:3457
2. Go to **Settings**
3. Toggle **Demo Mode** on or off

When toggling off, each agent's previous provider and model are restored automatically.

### Via the REST API

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/settings/demo-mode` | GET | Get current status |
| `/api/settings/demo-mode/enable` | POST | Enable demo mode |
| `/api/settings/demo-mode/disable` | POST | Disable demo mode |

### Via config file

Edit `.stoneforge/config.yaml`:

```yaml
demo_mode: true   # or false to disable
```

> **Note:** Editing the config file directly sets the flag but does not update agent provider/model settings. Use the dashboard toggle or API for a complete enable/disable.

## Limitations of the Free Model

The `minimax-m2.5-free` model is suitable for exploring Stoneforge but has limitations:

- **Smaller context window** — may struggle with large codebases
- **Lower capability** — less accurate than Claude, GPT-4, etc.
- **Rate limits** — free-tier models may have lower throughput
- **Best for learning** — not recommended for production workloads

## Upgrading to a Production Provider

1. Disable demo mode via dashboard Settings (restores previous configs)
2. Install your preferred provider CLI (e.g., `npm install -g @anthropic-ai/claude-code`)
3. Authenticate with the provider (e.g., run `claude`)
4. Reconfigure agents if needed:

```bash
sf agent register director --role director --provider claude-code
sf agent register e-worker-1 --role worker --provider claude-code
```

## Implementation Details

- **DemoModeService** (`packages/smithy/src/services/demo-mode-service.ts`): Manages enable/disable, saves/restores agent configs via SettingsService
- **Init command** (`packages/quarry/src/cli/commands/init.ts`): The `--demo` flag in `sf init` sets up demo mode at workspace creation
- **Settings routes** (`packages/smithy/src/server/routes/settings.ts`): REST API endpoints for status/enable/disable
- **Config flag**: `demo_mode` boolean in `.stoneforge/config.yaml`
- **Constants**: Provider = `opencode`, Model = `minimax-m2.5-free`

## Related Documents

- [Configuration Reference](../reference/config.md) — `demo_mode` config option
- [CLI Reference](../reference/cli.md) — `sf init --demo` flag
- [Platform Reference](../reference/platform.md) — dashboard settings page
