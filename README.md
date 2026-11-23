# Passport desktop downloads

Passport desktop runs a whole Passport on one computer: the server, the admin and member apps, an embedded database, a local inbox and a Stripe simulator. Nothing is shared and nothing can be reached from outside the machine.

This repository holds the builds only. Get them from [Releases](https://github.com/libatomic/passport-releases/releases).

**[Read the guide](docs/README.md):** install, using Passport, payments and the Stripe simulator, sample data, the command line, AI agents, CI and limits.

The Passport team's internal guide, which also covers the back office, cloning from hosted instances and development, is the [Passport desktop guide](https://claude.ai/artifact/16heESVFQoRsA3JAi44Pjh); it opens only for people it has been shared with.

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
bin/passport-desktop   the window (`passport up` opens it)
Helpers/               the Passport server
Resources/             the schema and web apps
```

Put `bin` on your `PATH`, or link `bin/passport` into a folder that is. Linux needs WebKitGTK 4.1 for the window; the command line needs nothing extra.

With Passport running, `passport trust` trusts its local certificate, which is only good for `.localhost` addresses. On Linux it goes into your browsers (Chrome, Chromium and Firefox, with `certutil` from `libnss3-tools` or `nss-tools`) and the system store, which asks for your password. On Windows it goes into your trusted roots after a confirmation. You can also point tools at the file `passport ca --path` prints (`curl --cacert`, `SSL_CERT_FILE`, `NODE_EXTRA_CA_CERTS`).

## Package managers

### Homebrew

```bash
# macOS (Apple silicon): the app, with passport on the PATH
brew install --cask libatomic/tap/passport

# Linux (x64, arm64): the headless command line
brew install libatomic/tap/passport
```

### Scoop (Windows x64, arm64)

```powershell
scoop bucket add libatomic https://github.com/libatomic/scoop-bucket
scoop install libatomic/passport
```

Each release updates the Homebrew cask and formula and the Scoop manifest. winget will follow once Windows builds are signed.

## Passport Studio

Passport Studio, the desktop app for building, testing and releasing Passport plugins, is published here too. Its releases are tagged `studio-v<version>`; Passport desktop's are `v<version>`.

| System | File |
|---|---|
| macOS 12 or later, Apple silicon | `Passport-Studio-<version>-macos-arm64.dmg` |
| Linux x64 / arm64 | `Passport-Studio-<version>-linux-x64.deb` / `-linux-arm64.deb`, or the `.AppImage`; full releases also have an `.rpm` |
| Windows x64 / arm64 | `Passport-Studio-<version>-windows-x64-setup.exe` / `-windows-arm64-setup.exe` |

Each file has a `.sha256` beside it. On a Mac, Homebrew installs it:

```bash
brew install --cask libatomic/tap/passport-studio
```

The macOS and Windows builds are not signed yet. Homebrew clears the macOS quarantine flag for you; from a downloaded `.dmg`, run `xattr -cr "/Applications/Passport Studio.app"` once. Windows asks before the installer's first run.

## atomic-cli

`atomic-cli`, the command line for managing a Passport instance, is published here too. Its releases are tagged `cli-v<version>`.

| System | File |
|---|---|
| macOS, Apple silicon / Intel | `atomic-cli_Darwin_arm64.tar.gz` / `atomic-cli_Darwin_x86_64.tar.gz` |
| Linux arm64 / x64 | `atomic-cli_Linux_arm64.tar.gz` / `atomic-cli_Linux_x86_64.tar.gz` |

Each release has an `atomic-cli_<version>_checksums.txt` file. Homebrew installs the newest full release on macOS and Linux:

```bash
brew install libatomic/tap/atomic-cli
```

For a release candidate, unpack the archive and put `atomic-cli` on your `PATH`.
