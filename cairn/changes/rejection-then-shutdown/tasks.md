---
cairn: tasks
change: rejection-then-shutdown
---

# Tasks

- [x] `SmtpResponse::first_complete_len`
- [x] Parse the first reply only, tolerate a 421 behind an error reply
- [x] Parse on EOF behind a complete reply, tolerate a 421 cut short
- [x] Tests in src/rfc5321/mail.rs
- [x] cargo fmt, clippy, feature matrix
- [ ] CHANGELOG entry under Unreleased
- [ ] Fold the delta into cairn/spec/command.md, log
