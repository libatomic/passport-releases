# Limits

- **The simulator is a model, not Stripe.** It covers the behaviour Passport relies on and fails loudly on anything else. Check billing changes against Stripe's own test mode before relying on them.
- **The database is not MySQL.** It is a compatible embedded engine without row locking: right for trying things and functional tests, wrong for load tests. An occasional "transaction has already been committed or rolled back" on a background job is a known artifact.
- **Nothing reaches it from outside.** Webhooks from real outside services cannot arrive. Simulated Stripe webhooks do, because they start inside the app.
- **Sizes.** Seeding makes at most 5,000 members and a copy at most 50 articles; a few hundred members is the practical size.
- **Platforms.** macOS on Apple silicon only (Intel Macs are not supported). Windows builds are not signed yet. The window is the newest part on Linux and Windows; headless is the most used there.
- **A copied instance's text is copied as written.** Member data, secrets and addresses are removed by rule, but if someone typed a real name or address into an article or template, it comes along.

## What leaves the computer

Nothing, unless you ask for it:

- Email never leaves: every message lands in the local inbox, at addresses under `.localhost`.
- Payments are simulated unless you connect an instance to your own Stripe test account.
- Article rendering uses the browser already on the computer, locally.
- Homebrew, Scoop and Passport Studio download from GitHub when you install or update.
