# Sample data

A new instance is empty. Three ways to fill one:

## Made-up members

```bash
passport seed demo --users 200 --plan Monthly=25% --past-due 5 --canceled 3
```

- People are made up, with addresses under the instance's own `.localhost` name, so no message can reach a real inbox.
- Subscriptions are created through the instance's own API against the Stripe simulator, so the server and the simulator agree. With no `--plan`, three in ten members subscribe.
- `--past-due` and `--canceled` happen the way they really do: a card that fails, and the simulator's clock moved past the renewal, which moves that instance's billing time forward by one to two and a half months.
- Members are marked as having verified their address, so a newsletter sent to them arrives. `--free-plan NAME` puts everyone not on a paid plan on a free one.
- The same `--seed` number makes the same people.
- It is not fast: about three minutes for 200 members and fifteen for 1,000. The most is 5,000.

## A demo publisher

`--demo` fills an instance with a made-up publisher, Quillfeather Press, so there is something to show and something to test distribution on. It is never done unless asked: tick **Fill it with a demo publisher** when creating an instance, or:

```bash
passport instance create quill --demo --users 50
passport seed demo --demo            # an existing instance
```

| What | Made |
|---|---|
| Podcast | "Ink and Air", with a cover; three episodes for everyone, with real audio, and a bonus episode for paying members |
| Newsletter | Two issues of "The Quill Weekly" and two supporters' longreads, the latest of each emailed to the members |
| Plans | Reader (free), Supporter (monthly and annual), Patron (monthly) |
| Member site | Its own stylesheet and a designed front page |
| Branding | Banner, icon, title, description, site and email colors and font |
| Templates | A newsletter email template and a podcast show-notes template |
| Members | As seeding makes them; those not on a paid plan join Reader |

Asking twice makes nothing new, and branding set by hand is never replaced.

## Copying an instance

```bash
passport clone demo --name demo-copy --articles 10 --users 100 --match-source
```

Makes a new local instance that looks like another one: its member site settings, categories, plans and prices (made again in the simulator), audiences' rules, templates, the latest articles (25 unless you say, 50 at most) with every author replaced, and the public files they use. No member data comes across, only counts per plan and status, which `--match-source` uses to make up members in the same proportions. Email credentials, Stripe settings and applications never come across.
