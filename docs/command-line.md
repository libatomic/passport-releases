# Command line

One `passport` command starts Passport and controls a running one. It is on your PATH after a Homebrew or Scoop install, is inside the macOS app at `Passport.app/Contents/Helpers/passport`, and is `bin/passport` in the Linux and Windows archives. `passport up` starts Passport; every other command talks to whichever Passport is already running. `passport COMMAND --help` describes each one.

```
passport up [--headless] [--detach] [--port N] [--panel ADDR] [--mcp ADDR]
passport down | status | logs [-f] [--tail N] | reset --yes

passport instance list | create SLUG [--demo [--users N]] | delete NAME --yes
passport instance open NAME [--member] [--print]     open signed in
passport instance session NAME [--env]               an admin API token and addresses
passport instance features NAME [KEY=on|off|unset|NUMBER]...

passport seed NAME [--demo] [--users N] [--plan NAME=N|P%]... [--free-plan NAME]
                   [--past-due N] [--canceled N] [--seed N]
passport clone SOURCE [--name SLUG] [--articles N] [--users N] [--match-source] [--seed N]
passport job [ID]                                    list or follow long work

passport stripe NAME [status]
passport stripe NAME use simulator | test-account
passport stripe NAME clock [advance 30d | to TIME | to next-renewal]
passport stripe NAME fail-next N | events | deliveries | objects [TYPE] | open

passport mail [--to ADDRESS] [--wait 30s] | mail show ID | mail clear | mail open
passport mcp [--http ADDR] | mcp install --client NAME [--project] [--print]
passport agents                                      instructions for an AI agent
passport trust | untrust | ca [--path]
passport version
```

`login`, `logout` and `whoami` connect to Passport's own back office and are for the Passport team.

- `--json` on any command prints JSON instead of text.
- `--data DIR` (or `PASSPORT_DATA`) uses a separate copy with its own data, so a test run never touches your own instances. There is one Passport per data folder; `passport up` when one is already running says so and exits 0.
- `passport up --headless --detach` returns once everything is ready, which is what a script waits on.
- `PASSPORT_PRINT_URLS=1` prints signed-in addresses instead of opening a browser.
- Exit codes: 0 done, 1 failed, 2 wrong usage, 3 Passport is not running, 4 not connected to the back office.

To use the instance's own API, `eval "$(passport instance session demo --env)"` sets the variables [atomic-cli](https://github.com/libatomic/homebrew-tap) reads, and `passport ca --path` is the certificate file to give tools that do not use the system's trust (`curl --cacert`, `NODE_EXTRA_CA_CERTS`, `SSL_CERT_FILE`).
