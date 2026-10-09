---
cairn: change
change: envelope-parameters
---

# Delta

## ADDED Requirements

### Requirement: Envelope parameters

`SmtpMessageSendOptions::mail_parameters` SHALL be sent on `MAIL FROM` and `SmtpMessageSendOptions::rcpt_parameters` on every `RCPT TO`, in the given order. Both default to empty, and the composite does not check the server capabilities.

## MODIFIED Requirements

## REMOVED Requirements
