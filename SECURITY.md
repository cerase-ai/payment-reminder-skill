# Security Policy

`payment-reminder-skill` is a skill of the Cerase platform: instructions an assistant follows when it reads a company's open invoices and writes payment reminders to its customers. It runs no code of its own; a defect here is an instruction that could lead an assistant to send a reminder the person did not approve, to the wrong customer or with a wrong amount.

It is maintained by Guidance Studio S.r.l.

## Supported versions

The release unit is the `main` branch. Cerase ships the same `SKILL.md` in its
appliance image and in the Cerase Marketplace. A fix ships as a new commit on
`main` and a new image; no earlier line is patched.

## Reporting a vulnerability

Email **tech@guidance.studio** with `payment-reminder-skill` in the subject. That
mailbox reaches the maintainers.

Do not open an issue, a discussion or a pull request for a suspected
vulnerability. This repository is public, so any of those is a disclosure.

Include as much of this as you have:

- what the defect is, and which instruction it is in
- the commit you tested
- steps to reproduce, or the conversation that shows it
- what an attacker gains, and what access they need to begin

Reports in English or Italian are equally welcome.

## Disclosure

Please hold details until a fixed image is published. We will tell you when
that happens, and we will credit you unless you prefer otherwise.

There is no paid bug bounty.
