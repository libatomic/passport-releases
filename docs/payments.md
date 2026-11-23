# Payments

Every instance is connected to a built-in Stripe simulator when it is created. There is nothing to set up: no Stripe account, no keys, no network. Live Stripe keys are refused.

- Plans, prices, coupons, subscriptions, invoices, saved cards and one-time payments all work, and the server receives Stripe's webhooks the moment something happens.
- The billing portal a member is sent to is a local page: cancel at the end of the period, resume, change card.
- The member app's payment forms show simulated card fields, marked **SIM**, and a simulated wallet button. A card that needs authentication opens a dialog with **Complete** and **Fail**. Bank account payments are not simulated.

## Test cards

Any future expiry date and any CVC.

| Card number | What happens |
|---|---|
| 4242 4242 4242 4242 | Succeeds |
| 4000 0000 0000 0002 | Declined |
| 4000 0000 0000 9995 | Declined, insufficient funds |
| 4000 0025 0000 3155 | Asks for authentication, then succeeds |
| 4000 0000 0000 0341 | Saves, then fails when charged (for testing renewal failures) |

## The dashboard

`https://stripe.localhost`, or the card icon in the app, shows customers, subscriptions, invoices, payments, products and prices, every event and each webhook delivery with the instance's answer, and the account's clock. From there you can send an event again or make the next charges fail.

```bash
passport stripe demo                 # the instance's simulator account and clock
passport stripe demo events          # recent events
passport stripe demo deliveries      # webhook deliveries and answers
passport stripe demo fail-next 2     # the next two charges fail
passport stripe demo open            # the dashboard in the browser
```

## Moving time

Each instance's simulator has its own clock, which moves forward only:

```bash
passport stripe demo clock advance 30d
passport stripe demo clock to next-renewal
```

Everything due in between happens in order, as it would at Stripe: trials ending, renewal reminders, renewals, retries after a failed payment, cancellation. The server is told about each, and the emails it sends land in the inbox. A month of renewals takes a second or two. The clock stays moved, so an instance whose clock was advanced is no longer at today's date for billing.

## A real Stripe test account

For what the simulator does not model, an instance can use a Stripe account in test mode: `passport stripe demo use test-account` (or **Disconnect**, then **Connect with Stripe**, under Payments in its admin app) and enter the account's test keys. `passport stripe demo use simulator` switches back. Stripe cannot send webhooks to your computer, so on a test account a subscription does not update after it is created.

The simulator covers the Stripe behaviour Passport relies on and fails loudly on anything else, but it is a second implementation: check billing changes against Stripe's own test mode before relying on them.
