# Raisely Claude plugin

Connect [Claude](https://claude.ai) to [Raisely](https://raisely.com). This plugin ships the Raisely MCP server so the Claude agent can query campaigns, donations, supporters, and the rest of your Raisely data straight from chat.

## What's inside

- `.mcp.json`: registers the hosted Raisely MCP server at `https://mcp.raisely.com/mcp`.
- `.claude-plugin/plugin.json`: plugin manifest.
- `./skills`: Set of skills

The MCP server uses OAuth, so the first time you use it Claude will open a browser tab to sign you in to Raisely.

## Install

### From the Claude Marketplace

Search for `Raisely` in the Claude marketplace panel and click Install. Claude will register the MCP server automatically.

### From source (local development)

Open Claude Code and run:

`/plugin`

Follow the instructions to install a plugin. When asked for the path, use the plugin directory full path.

## Use it

Once connected, ask the agent things like:

- "List my active campaigns in Raisely."
- "Show the top 10 donations to the Spring Appeal this week."
- "Find supporters who signed up in the last 30 days but haven't donated."

The agent will pick the right Raisely MCP tool and walk you through the OAuth prompt the first time.

## Contributing

This plugin will grow to include Raisely-specific skills and agents. PRs welcome at [github.com/raisely/claude-plugin](https://github.com/raisely/claude-plugin).

## License

MIT
