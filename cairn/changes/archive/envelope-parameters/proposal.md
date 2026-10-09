---
cairn: change
id: envelope-parameters
status: landed
created: 2026-10-09
---

# Envelope parameters on the composite send

## Why

pimalaya/himalaya#775 asks for delivery status notifications (RFC 3461) on `himalaya smtp send`. The DSN parameters already exist (`rfc3461::parameter`) and the single steps `mail()` and `rcpt()` take them, but `SmtpMessageSend` always sent empty parameter lists.

## What

- `SmtpMessageSendOptions` gains `mail_parameters` and `rcpt_parameters`, sent on `MAIL FROM` and on every `RCPT TO`.
- Generic ESMTP parameters rather than DSN fields, so `SIZE`, `BODY` or `SMTPUTF8` ride the same path.
- No capability check: the caller holds the EHLO capabilities.
