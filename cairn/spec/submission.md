---
cairn: spec
capability: submission
status: current
---

# Submission

`SmtpMessageSend`, the composite that submits one message: `MAIL FROM`, one `RCPT TO` per forward path, then `DATA`. Its callers are mail clients handing a message to their submission server, not relays, which is why it may touch the content it transmits.

## Requirement: Blind recipients stay blind

`SmtpMessageSend` SHALL remove every `Bcc` field from the header section of the transmitted message (RFC 5322 3.6.3), matching the name case-insensitively and with the obsolete whitespace before the colon, removing its folded lines too, and copying everything after the first empty line verbatim. The forward paths are untouched, so the blind recipients still receive the message.

`SmtpMessageSendOptions::keep_bcc` SHALL transmit the message as given, for a caller whose bytes must reach the server unchanged.

### Scenario: A message with a blind recipient
- GIVEN a message carrying `Bcc: c@x` and default options
- WHEN it is sent
- THEN the `DATA` payload carries no `Bcc` field, and a `Bcc:` line in the body is left alone

## Requirement: Envelope parameters

`SmtpMessageSendOptions::mail_parameters` SHALL be sent on `MAIL FROM` and `SmtpMessageSendOptions::rcpt_parameters` on every `RCPT TO`, in the given order (RFC 5321 4.1.1.2, 4.1.1.3). Both default to empty. The composite does not check the server announced the matching extension: the caller holds the EHLO capabilities and decides. A caller needing different parameters per recipient drives the single steps instead.

### Scenario: A delivery status notification request
- GIVEN `RET=HDRS` and `ENVID=abc123` as MAIL parameters and `NOTIFY=SUCCESS,FAILURE` as RCPT parameters
- WHEN the message is sent
- THEN the server reads `MAIL FROM:<a@x> RET=HDRS ENVID=abc123` and `RCPT TO:<c@x> NOTIFY=SUCCESS,FAILURE`
