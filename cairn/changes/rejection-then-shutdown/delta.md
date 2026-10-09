---
cairn: delta
change: rejection-then-shutdown
---

# Delta

## ADDED Requirements

### Requirement: An error reply followed by a 421 is the reply
`SmtpCommandSend` SHALL parse the first complete reply in its read buffer and return it. When that reply is an error and the bytes behind it are a 421 reply, complete or cut short by the stream closing, the 421 SHALL be dropped and logged. Any other bytes behind the reply SHALL fail the command as an unparseable response, naming them. A stream closing behind a complete reply SHALL parse that reply; one closing before it SHALL fail as EOF.

#### Scenario: Rejection then shutdown in one read
- GIVEN `MAIL FROM` answered with `501 5.1.7 Bad sender address syntax` and `421 4.7.0 Error: too many errors` in one read
- WHEN the reply is parsed
- THEN the command fails with the 501 rejection

#### Scenario: Shutdown cut short by the close
- GIVEN the same 501 followed by `421 4.7.0 mta1`, then EOF
- WHEN the stream closes
- THEN the command fails with the 501 rejection

## MODIFIED Requirements

## REMOVED Requirements
