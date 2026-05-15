> ## Documentation Index
> Fetch the complete documentation index at: https://docs.beaconcha.in/llms.txt
> Use this file to discover all available pages before exploring further.

# AI Integration

> Connect AI assistants and LLMs to beaconcha.in documentation for accurate, up-to-date answers about Ethereum staking and our API.

Connect your AI assistant to beaconcha.in documentation for accurate, contextual answers about Ethereum staking, validators, and our API.

## llms.txt (Universal)

For any AI agent or LLM that follows the [llms.txt standard](https://llmstxt.org/), fetch our complete documentation index:

```
https://docs.beaconcha.in/llms.txt
```

This file is auto-generated and contains links to all documentation pages with descriptions, including:

* **V1 & V2 API Reference** — All endpoints with parameter descriptions and usage notes
* **API Guides** — Authentication, pagination, rate limits, and dashboard management
* **Use Cases** — Rewards calculations, performance monitoring, queue tracking
* **Validator Dashboard** — Configuration, groups, slot visualization, BeaconScore
* **FAQs** — Deposits, withdrawals, validator queues, and staking concepts
* **Legal** — Terms, privacy, and BeaconScore license

<Tip>
  Use `llms.txt` when your AI tool doesn't support MCP, or when you need to provide your AI assistant with a complete list of available documentation.
</Tip>

***

## skill.md (For AI Agents)

For AI agents and assistants that support skill files or need comprehensive capability documentation:

```
https://docs.beaconcha.in/skill.md
```

This file provides a complete reference designed specifically for AI agents, including:

* **Capabilities** — Overview of all beaconcha.in APIs and monitoring features
* **Skills** — Detailed instructions for validator lookups, rewards calculations, performance monitoring, and more
* **Workflows** — Step-by-step guides for common tasks like tax reporting and validator monitoring
* **Integration** — Connections with Lido, Rocket Pool, MEV-Boost, and Ethereum clients
* **Context** — Important background on Ethereum staking architecture and BeaconScore metrics

<Tip>
  The skill.md format is ideal for AI agents in development environments (like Cursor) that can read and follow structured capability documentation to assist with beaconcha.in integration tasks.
</Tip>

***

## MCP Server (For Supported Tools)

For AI tools that support Model Context Protocol (MCP), connect directly to our documentation server for richer integration:

```
https://docs.beaconcha.in/mcp
```

### What is MCP?

Model Context Protocol (MCP) is an open standard that enables AI assistants to securely connect to external data sources and tools. By using our MCP server, you can give your AI assistant direct access to beaconcha.in documentation, enabling it to provide more accurate and contextual answers.

### Setting Up MCP

<Tabs>
  <Tab title="Cursor IDE">
    Add the following to your Cursor MCP configuration (`.cursor/mcp.json`):

    ```json theme={null}
    {
      "mcpServers": {
        "beaconchain-docs": {
          "url": "https://docs.beaconcha.in/mcp"
        }
      }
    }
    ```
  </Tab>

  <Tab title="Claude Desktop (macOS)">
    Edit `~/Library/Application Support/Claude/claude_desktop_config.json`:

    ```json theme={null}
    {
      "mcpServers": {
        "beaconchain-docs": {
          "url": "https://docs.beaconcha.in/mcp"
        }
      }
    }
    ```
  </Tab>

  <Tab title="Claude Desktop (Windows)">
    Edit `%APPDATA%\Claude\claude_desktop_config.json`:

    ```json theme={null}
    {
      "mcpServers": {
        "beaconchain-docs": {
          "url": "https://docs.beaconcha.in/mcp"
        }
      }
    }
    ```
  </Tab>

  <Tab title="Other Tools">
    For other MCP-compatible tools, use the server URL `https://docs.beaconcha.in/mcp` and follow your tool's specific configuration instructions.
  </Tab>
</Tabs>

### Available via MCP

Once connected, the MCP server provides access to:

* **API Documentation** — Complete reference for the beaconcha.in API v1 and v2
* **Validator Dashboard Guides** — How to use and configure validator dashboards
* **FAQs** — Common questions about deposits, withdrawals, and validator operations
* **Notifications & Monitoring** — Setting up alerts and monitoring for your validators

***

## Example Use Cases

<CardGroup cols={2}>
  <Card title="API Integration Help" icon="code">
    Ask your AI assistant to help you integrate the beaconcha.in API into your application with accurate, up-to-date documentation.
  </Card>

  <Card title="Validator Monitoring" icon="eye">
    Get guidance on setting up notifications and monitoring for your validators directly in your development environment.
  </Card>

  <Card title="Dashboard Configuration" icon="chart-line">
    Learn how to configure and customize your validator dashboard with contextual assistance.
  </Card>

  <Card title="Troubleshooting" icon="wrench">
    Diagnose issues with deposits, withdrawals, or validator performance using documentation-backed answers.
  </Card>
</CardGroup>

***

## Need Help?

If you have questions about AI integration or encounter any issues, reach out to us on [Discord](https://dsc.gg/beaconchain) or [X](https://x.com/beaconcha_in).
