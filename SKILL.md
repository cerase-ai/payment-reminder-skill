---
name: payment-reminder
description: "Use when the person asks to chase unpaid or overdue invoices, to remind a customer of a payment, or for the list of customers who owe money. It reads the open customer invoices from the accounting connector, or from the file or list the person gives, and searches the person's mail for a customer's address the data lacks. It lists the invoices by customer with their due dates, amounts still owed and days late, and after the person chooses whom to remind writes one reminder per customer listing all their open invoices, in a tone that follows how late they are. Each reminder is created as a draft or sent only once the person approves that call; no amount, invoice or bank detail is invented."
---

# Payment reminder

You turn the open invoices into reminders the person can send: one per customer, with every invoice that customer still owes, firm in proportion to how late it is.

## Where the open invoices are

1. With the Odoo connector among yours, read the posted customer invoices not yet paid and already due:

```
call_recipe("odoo-direct.search_records", {
  "model": "account.move",
  "domain": [["move_type", "=", "out_invoice"], ["state", "=", "posted"], ["payment_state", "in", ["not_paid", "partial"]], ["invoice_date_due", "<", "<today, YYYY-MM-DD>"]],
  "fields": ["name", "partner_id", "invoice_date", "invoice_date_due", "amount_total", "amount_residual", "currency_id"],
  "order": "invoice_date_due asc",
  "limit": 200
})
```

   The customer's address: `call_recipe("odoo-direct.read_record", {"model": "res.partner", "record_id": <the partner_id number>, "fields": ["name", "email"]})`.
2. Without it, the file or sheet the person names: a workspace file with `call_recipe("cerase-docreader.read_document", {"path": "<its path>"})`, a Google Sheet with `call_recipe("google-workspace.getGoogleSheetContent", {"spreadsheetId": "<id>", "range": "<the sheet's name>"})`, or the list the person pastes in the chat. If there is none of these, ask where the open invoices are, in one question.
3. A customer's address that the data does not carry: say so in the list, then look for it in their last mail with `call_recipe("gmail.search_emails", {"query": "from:<the customer's domain or name>", "maxResults": 5})`, or ask the person.

## The list

Show the person, before writing any reminder: each customer with their open invoices (number, due date, amount still owed, days late counted from today), the customer's total, and the address the reminder would go to. Add up every total with `call_recipe("cerase-calc.sum", {"values": ["<amount>", "<amount>"]})`: never add one up yourself. Ask whom to remind; «all of them» is an answer.

## The reminders

One mail per customer, in the language they write in, by the rules of the `email-drafting` skill. It lists every open invoice of that customer with its number, date, due date and amount still owed, and the total. The tone follows the oldest invoice:

- up to 30 days late, or a first reminder: cordial, assumes an oversight, asks when the payment will be made;
- 31 to 60 days, or after an earlier reminder the mail shows: firm, asks for the payment or a date within a few days;
- over 60 days: formal, asks for the payment by a date the person chooses, and says the person will follow up by phone.

Interest, penalties, a stop of supplies or legal action are written only when the person asks for them. Bank details go in only when the invoice data or the person gives them, copied exactly. An invoice partly paid is listed for the amount still owed.

Then, as the person chooses, for each customer: a Gmail draft, `call_recipe("gmail.draft_email", {"to": ["<address>"], "subject": "<subject>", "body": "<the reminder>"})`, or the send, `call_recipe("gmail.send_email", {"to": ["<address>"], "subject": "<subject>", "body": "<the reminder>"})`. Each call waits for the person's approval, and that approval is their confirmation: do not also ask for it in the chat. Say in one line which reminders wait for approval.

## Hard rules

- Nothing is sent, and no draft is created, before the person has seen the list and chosen whom to remind.
- Every number, date and amount comes from the invoice data and every total from `cerase-calc`; an invoice the data does not show is never added.
- A customer who says they have paid, or whose invoice the data shows as paid, is not reminded: tell the person instead.
