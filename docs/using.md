# Using Passport

## Instances

An instance is one publication. Create one from the control panel with a short name, for example `demo`, or with `passport instance create demo`:

| What | Address |
|---|---|
| Admin app | `https://demo.localhost/admin` |
| Member app | `https://demo.localhost/member` |
| API | `https://demo.localhost/api/1.0.0` |
| Inbox, for every instance | `https://mail.localhost` |
| Stripe simulator dashboard | `https://stripe.localhost` |

If something else already uses port 443, Passport uses 8443 and every address carries `:8443`. Use the addresses the control panel or `passport status` shows rather than building them.

- The admin is `admin@demo.localhost` with the password `passport`, unless you chose others. The **Admin** button opens the admin app already signed in, as does `passport instance open demo`.
- **Edit** changes the title and the instance's features. Features that make no sense on one computer are not offered.
- `.localhost` addresses work in every current browser with no setup; they always point at your own computer.

## The inbox

Every email any instance sends, including sign-in links, receipts and newsletters, appears at `https://mail.localhost` and nowhere else. Nothing is delivered outside. From the command line:

```bash
passport mail                          # recent messages
passport mail --to someone@demo.localhost --wait 30s   # wait for one
passport mail show ID                  # one message and its links
```

## The control panel

The window shows the status of each part, the instances, the certificate, and the log. **Restart** restarts the server; quitting the app stops everything it started. Running headless, `passport up --headless --panel 127.0.0.1:7077` serves the same panel as a page at that address.

## Plugins

[Passport Studio](https://github.com/libatomic/passport-releases#passport-studio) finds a running Passport by itself and can install and test draft plugins on any local instance, with no sign-in.

## Data

Everything Passport stores is in its data folder:

| System | Folder |
|---|---|
| macOS | `~/Library/Application Support/Passport` |
| Linux | `~/.config/Passport` |
| Windows | `%AppData%\Passport` |

**Reset data** in the control panel (or `passport reset --yes`) deletes every instance and starts clean. To run a separate copy that never touches these instances, set `PASSPORT_DATA` (or `--data DIR`) to another folder; only one Passport runs per data folder.

## Troubleshooting

- **The browser warns about the certificate.** Choose **Trust certificate**, or run `passport trust`, then reload.
- **Addresses need `:8443`.** Something else holds port 443. That is fine; use the address Passport shows.
- **An email is missing.** Look in `https://mail.localhost`. Nothing is sent outside.
- **Something is stuck.** The control panel's log shows the server's log (`passport logs -f` from a terminal). **Restart**, or quit and reopen.
- **The window misbehaves on Linux or Windows.** It is the newest part there. Run `passport up --headless --panel 127.0.0.1:7077` and use the panel in your browser instead.
