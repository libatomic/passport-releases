# Passport desktop downloads

Passport desktop runs a whole Passport on one computer: the server, the admin and member apps, an embedded database, a local inbox and a Stripe simulator. Nothing is shared and nothing can be reached from outside the machine.

This repository holds the builds only. Get them from [Releases](https://github.com/libatomic/passport-releases/releases).

| System | File |
|---|---|
| macOS 13 or later, Apple silicon | `Passport-<version>-arm64.dmg` |
| Linux x64 | `passport-<version>-linux-amd64.tar.gz` |
| Linux arm64 | `passport-<version>-linux-arm64.tar.gz` |
| Windows x64 | `passport-<version>-windows-amd64.zip` |
| Windows arm64 | `passport-<version>-windows-arm64.zip` |

Each release has a `SHA256SUMS` file to check a download against:

```bash
sha256sum --check --ignore-missing SHA256SUMS
```

## macOS

Open the disk image and drag Passport to Applications. The command line is inside the app at `Passport.app/Contents/Helpers/passport`.

## Linux and Windows

Unpack the archive anywhere and keep its folders together:

```
bin/passport           the command line; `passport up --headless` runs Passport
bin/passport-desktop   the window (a preview on these systems)
Helpers/               the Passport server
Resources/             the schema and web apps
```

Put `bin` on your `PATH`, or link `bin/passport` into a folder that is. Linux needs WebKitGTK 4.1 for the window; the command line needs nothing extra.

Trusting Passport's local certificate automatically is macOS only for now. On Linux and Windows, run `passport ca --path` and import that file into your browser, or point your tools at it (`curl --cacert`, `SSL_CERT_FILE`, `NODE_EXTRA_CA_CERTS`).

## Package managers

Homebrew (macOS and Linux) and winget or Scoop (Windows) are coming.
