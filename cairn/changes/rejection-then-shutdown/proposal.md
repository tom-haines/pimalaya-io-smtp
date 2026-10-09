---
cairn: change
id: rejection-then-shutdown
status: active
created: 2026-10-09
---

# An error reply followed by a 421 surfaces as the rejection

## Why

A server giving up on a session answers the offending command with an error reply, then sends `421 ... closing transmission channel` and closes (RFC 5321 section 4.2.1). Migadu does this after a bad `MAIL FROM` (`421 4.7.0 Error: too many errors`). Both replies usually land in one read, and `SmtpCommandSend` parsed its whole buffer as one reply, so the caller got a parse error in place of the `501` it needed to act on. When the reads split the 421 and the stream closes behind its first part, the caller got `Eof` instead.

## What

- src/rfc5321/types/response.rs: `SmtpResponse::first_complete_len` finds where the first reply ends.
- src/send.rs: the parse step takes the first reply only. Bytes behind it are tolerated when the reply is an error and they are a 421, complete or cut short by the stream closing; anything else fails the command, naming the bytes. EOF behind a complete reply parses it instead of failing.
- src/rfc5321/mail.rs: tests for the one-read, split-read and cut-short cases, and for trailing bytes that are not a 421.

A 421 behind a success reply is still refused: no server is known to send one there, and dropping it would hide a desynchronised exchange.

The behaviour belongs to the command exchange, which has no spec file yet: the fold creates cairn/spec/command.md.
