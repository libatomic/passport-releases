# Install

Builds are published in [libatomic/passport-releases](https://github.com/libatomic/passport-releases/releases). The package managers below install from there; nothing needs a credential.

| System | Install |
|---|---|
| macOS 13 or later, Apple silicon | `brew install --cask libatomic/tap/passport` |
| Linux x64, arm64 | `brew install libatomic/tap/passport`, or unpack `passport-<version>-linux-<arch>.tar.gz` |
| Windows x64, arm64 | `scoop bucket add libatomic https://github.com/libatomic/scoop-bucket`, then `scoop install libatomic/passport`; or unpack `passport-<version>-windows-<arch>.zip` |

On macOS use `--cask`: the app goes into Applications and the `passport` command onto your PATH. Homebrew adds the `libatomic/tap` tap by itself the first time.

Each release has a `SHA256SUMS` file to check a download against.

## Unpacked archives (Linux, Windows)

An archive unpacks to one folder:

```
bin/passport           the command line; `passport up` starts Passport
bin/passport-desktop   the window
Helpers/               the Passport server
Resources/             the schema and the web apps
```

Keep the folders together: `bin/passport` finds `Helpers` and `Resources` beside it, also through a link on your PATH. The window on Linux needs GTK 3 and WebKitGTK 4.1 (`libwebkit2gtk-4.1-0` on Debian and Ubuntu); the command line needs nothing extra. Windows needs the WebView2 runtime, which current Windows already has.

## Signing

The macOS app is signed and notarized by Apple, so it opens without a warning. Windows builds are not signed yet: SmartScreen asks before the first run (**More info**, then **Run anyway**).

## First launch

1. Open **Passport** (Applications on macOS, the Start menu on Windows), or run `passport up`. `passport up --headless` runs it with no window.
2. The control panel shows each part starting: database, schema, mail, Stripe simulator, browser, server, local HTTPS.
3. Choose **Trust certificate** once (or run `passport trust`). Passport makes its own certificate authority, which can only vouch for `.localhost` addresses, so trusting it cannot affect any real website. Without it, browsers warn on every local page.
   - macOS asks for your password or Touch ID.
   - Linux adds it to Chrome, Chromium and Firefox (this needs `certutil`: `libnss3-tools` on Debian and Ubuntu, `nss-tools` on Fedora) and to the system store, which asks for your password. Without a way to ask, it prints the two commands to run.
   - Windows asks you to confirm adding it to your trusted roots. No administrator rights are needed.
4. Choose **New instance**.

Passport renders articles for email with the Chrome (or Chromium, or Edge) already on the computer. Without one, Passport runs, and that one feature is off.

## Updating

```bash
brew upgrade --cask passport      # macOS
brew upgrade passport             # Linux
scoop update passport             # Windows
```

## Uninstalling

| | Removes the app | Also removes your data |
|---|---|---|
| macOS | `brew uninstall --cask passport` | add `--zap` |
| Linux | `brew uninstall passport` | delete `~/.config/Passport` |
| Windows | `scoop uninstall passport` | delete `%AppData%\Passport` |

Run `passport untrust` first if you trusted the certificate.
