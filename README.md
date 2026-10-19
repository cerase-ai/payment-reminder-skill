# payment-reminder-skill

A Cerase skill that turns a company's open customer invoices into payment reminders: one per customer, listing every invoice that customer still owes, firm in proportion to how late it is, created as a draft or sent only after the person approves it.

## What the assistant does

1. Reads the open, overdue customer invoices from Odoo with `odoo-direct.search_records` on `account.move` (the customer's address with `odoo-direct.read_record`), or from the file or sheet the person names.
2. Shows the list by customer with due dates, amounts still owed, days late and totals from `cerase-calc.sum`, and asks whom to remind.
3. Writes one reminder per chosen customer, and creates it with `gmail.draft_email` or sends it with `gmail.send_email`; each call waits for the person's approval.

## What it never does

- Sends or drafts anything before the person has seen the list and chosen.
- Invents an amount, an invoice or bank details, or adds interest, penalties or legal action the person did not ask for.

## Requirements

Odoo (`odoo-direct`) or a file or sheet with the open invoices; Gmail (`gmail`) to draft or send; the calculator (`cerase-calc`).

## Files

| File | Purpose |
|---|---|
| `SKILL.md` | The instructions the assistant loads. |
| `cerase.json` | Marketplace manifest: namespace `studio.guidance`, name `payment-reminder`, display name, description, licence. |
| `i18n.yaml` | Italian display name and description for the Marketplace; not sent to the assistant. |
| `LICENSE` | MIT licence text. |

## Installation

Published in the Cerase Marketplace as `studio.guidance/payment-reminder`.
A Cerase appliance also ships it in its image.

## License

MIT. See [LICENSE](LICENSE).
