---
cairn: log
change: envelope-parameters
landed: 2026-10-09
---

# Envelope parameters on the composite send

Requested in pimalaya/himalaya#775 by @snrmwg: a way to ask for delivery status notifications (RFC 3461) through `himalaya smtp send`. The DSN parameter constructors and the single steps already supported it; the composite `SmtpMessageSend` hard-coded empty parameter lists.

**`SmtpMessageSendOptions`** (message.rs) gains `mail_parameters`, sent on `MAIL FROM`, and `rcpt_parameters`, sent on every `RCPT TO`. They are plain `SmtpParameter` lists rather than DSN fields, so any ESMTP extension parameter fits. One list serves all recipients; per-recipient parameters stay with the single steps. The composite checks no capability, the EHLO lines belonging to the caller.

**Breaking**: two new public fields, so a struct literal needs `..Default::default()`.

Verified: 106 unit tests green, one new over the written MAIL FROM and RCPT TO lines. Clippy clean, the no-default-features build passes.

Spec updated: `submission` (ADDED: "Envelope parameters").
