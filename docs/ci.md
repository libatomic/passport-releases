# CI

Passport runs headless on macOS, Linux and Windows runners, x64 and arm64, so a CI job can start a clean Passport, build a known state, run tests against it and throw it away. Linux runners are the cheapest.

```yaml
jobs:
  sandbox:
    runs-on: ubuntu-24.04
    env:
      PASSPORT_DATA: ${{ runner.temp }}/passport
      PASSPORT_PORT: "9443"
    steps:
      - uses: actions/checkout@v4
      - name: Install Passport
        run: |
          eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
          brew install libatomic/tap/passport
          echo "$(brew --prefix)/bin" >> "$GITHUB_PATH"
      - run: passport up --headless --detach
      - run: passport instance create under-test --demo --users 100
      - name: Tests
        run: |
          export NODE_EXTRA_CA_CERTS="$(passport ca --path)"
          eval "$(passport instance session under-test --env)"
          npm test
      - name: Keep the evidence
        if: failure()
        run: |
          passport logs --tail 2000 > passport.log
          passport --json mail > mail.json
          passport --json stripe under-test deliveries > webhooks.json
      - uses: actions/upload-artifact@v4
        if: failure()
        with:
          name: passport
          path: |
            passport.log
            mail.json
            webhooks.json
      - run: passport down
        if: always()
```

- **Getting Passport.** Homebrew on Linux and macOS runners (`brew install --cask libatomic/tap/passport` on macOS), Scoop on Windows, or download and unpack the archive from [Releases](https://github.com/libatomic/passport-releases/releases).
- **Port.** Port 443 needs privileges on Linux; `PASSPORT_PORT` picks another, and every address then carries it. Read addresses from `passport status --json` rather than building them.
- **Certificates.** Point tools at `passport ca --path` (`NODE_EXTRA_CA_CERTS`, `SSL_CERT_FILE`, `curl --cacert`). On a Linux runner, `passport trust` also works, because hosted runners have passwordless sudo.
- **Speed.** A fresh data folder each run gives a clean Passport. Making up 100 members takes a minute or two; 1,000 about fifteen.
- **Agents.** Add `--mcp 127.0.0.1:7078` to `passport up` and point an agent in the job at that address, or have it run `passport mcp`.

Passport's own release workflow starts each Linux and Windows build headless, creates an instance, fetches it over HTTPS and stops it, on every release. The job above uses the same commands but has not been run as written; adjust it to your repository.
