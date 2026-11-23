# Passport desktop guide

Passport desktop runs a whole Passport on one computer: the server, the admin and member apps, a database, a local email inbox and a Stripe simulator. Nothing is shared, nothing reaches the network, and nothing can be reached from outside the machine. Use it for demos, trying things out, testing templates and plugins, and automated tests, by a person, a CI job or an AI agent.

It runs on macOS (Apple silicon), Linux and Windows (x64 and arm64), with a window or headless.

| Page | What it covers |
|---|---|
| [Install](install.md) | Homebrew, Scoop or a download; first launch; trusting the certificate; uninstalling |
| [Using Passport](using.md) | Instances and their addresses, the inbox, the control panel, where data lives, troubleshooting |
| [Payments](payments.md) | The built-in Stripe simulator, test cards, the dashboard, moving time, using a real Stripe test account |
| [Sample data](sample-data.md) | Made-up members and subscriptions, the demo publisher, copying an instance |
| [Command line](command-line.md) | Every `passport` command |
| [AI agents](agents.md) | Connecting Claude Code, Claude, Codex, Cursor and others over MCP |
| [CI](ci.md) | Running Passport headless in a CI job |
| [Limits](limits.md) | What it is not, and what leaves the machine |

Quick start on a Mac:

```bash
brew install --cask libatomic/tap/passport
```

Open **Passport** from Applications, choose **Trust certificate** once, then **New instance**.
