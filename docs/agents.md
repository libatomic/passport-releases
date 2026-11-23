# AI agents

Passport includes an MCP (Model Context Protocol) server, so an AI agent can run the sandbox the way a person runs it from the app and the command line: make instances, fill them, read the inbox, move billing time, and check what happened. Passport has to be running already; an agent cannot start it.

## Connecting

| Agent | Setup |
|---|---|
| Claude Code | `passport mcp install --client claude`; add `--project` to write `.mcp.json` in the repository for the whole team |
| Claude (desktop app) | `passport mcp install --client claude-desktop` |
| Codex | `passport mcp install --client codex` |
| Cursor | `passport mcp install --client cursor`, or `--project` for the repository's `.cursor/mcp.json` |
| VS Code, Gemini CLI, Windsurf and others | `passport mcp install --print` gives the entry to add by hand |
| Claude on the web, or any hosted agent | Not possible: a hosted agent cannot reach a server on your computer. Run Passport in a CI job beside the agent instead |

The installer changes only its own entry in the agent's configuration and keeps a copy of the file as it was (`.before-passport`) the first time.

There are two ways to connect, and both are standard MCP:

- **A local command**, `passport mcp`, which the agent's app starts. It finds the running Passport afresh on every call, so it survives Passport restarting. This is what the installer sets up.
- **A local address**, `passport mcp --http 127.0.0.1:7078` (or `passport up --headless --mcp 127.0.0.1:7078`), on this computer only and protected by the token in `local-api.token` in the data folder. For clients that prefer an address, or several agents sharing one sandbox.

For agents that read a project instructions file, `passport agents` prints the same guidance the MCP server gives (how to find addresses, the test cards, how to read mail, how time works, what not to do) to paste into `AGENTS.md` or `CLAUDE.md`.

## The tools

| Group | Tools | What an agent does with them |
|---|---|---|
| Status | `status`, `logs_tail`, `ca_info`, `help` | Check everything is running, learn the real addresses, read logs when a step fails |
| Instances | `instance_list`, `instance_create`, `instance_delete`, `instance_features_get`, `instance_features_set` | Make and configure publications |
| Sessions | `instance_session` | A signed-in admin address for browser tests, and an access token with the API address for API tests |
| Sample data | `clone_start`, `seed_start`, `job_wait`, `job_list` | Build a realistic starting state; these return a job, and `job_wait` waits for it |
| Mail | `mail_list`, `mail_get`, `mail_wait`, `mail_clear` | Read what was sent; `mail_wait` waits for a message to an address and returns its links, which is how an agent completes sign-up and sign-in |
| Billing | `stripe_state`, `stripe_objects`, `stripe_events`, `stripe_deliveries`, `stripe_clock_advance`, `stripe_fail_next`, `stripe_event_resend` | Inspect simulated billing, move time, force failures |
| Reset | `reset` | Wipe everything |

Tools that only read are marked so. The three that destroy something (`instance_delete`, `mail_clear`, `reset`) are marked destructive, so a client can ask first, and they refuse unless called with `confirm: true`. Two prompts ship as worked examples: `smoke_test_instance` and `subscription_lifecycle`.

The instance's own API (members, plans, articles, subscriptions) is not repeated here: `instance_session` returns the values [atomic-cli](https://github.com/libatomic/homebrew-tap) accepts, so an agent can run atomic-cli's own MCP server beside this one, pointed at the local instance.

## An example run

A subscription's life, with no person involved:

1. `status`, to confirm the sandbox is up.
2. `instance_create` with `demo: true`, then `job_wait`.
3. In a browser tool, sign up as a new member on the member app.
4. `mail_wait` for that address; follow the verification link it returns.
5. Subscribe to a plan with the 4242 test card.
6. `stripe_clock_advance` to the next renewal; confirm through the API that it renewed and, with `mail_list`, that the receipt went out.
7. `stripe_fail_next`, advance again, and confirm the member is past due and the payment-failed email was sent.

## Safety

The MCP server listens on this computer only. Everything an agent can change is local and disposable: mail cannot leave, and payments are simulated. An agent cannot switch an instance to a real Stripe account.
