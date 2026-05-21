# Security

This file tracks fixed security issues and known open problems in this LinBPQ tree.

## Fixed

- [x] Critical: HTTP session cookies are only accepted when `BPQSessionCookie=N...` resolves to a live session.
- [x] Critical: APRS message POST requires local access or a valid session.
- [x] Critical: APRS message POST rejects missing callsign input safely.
- [x] High: RHP websocket stream open requests require an authenticated secure websocket session.
- [x] High: Rig and RHP websocket control paths no longer treat cookie presence alone as authentication.
- [x] High: HTTP session keys use OS random bytes instead of time-derived values.
- [x] High: Node API tokens use OS random bytes instead of `rand()`.
- [x] High: Mail API tokens use OS random bytes instead of `rand()`.
- [x] High: Beacon form destination, file, and text writes use bounded copies.
- [x] High: Beacon form handling rejects invalid ports before using the port slot.

## Known Problems

- [ ] Full object checks for `HTTPcode.c`, `APRSCode.c`, and `nodeapi.c` are blocked by the existing `Rigresource.h` include case mismatch.
- [ ] `mailapi.c` object compile is blocked by the existing missing `dbghelp.h` include.
- [ ] `RHP.c` still emits an existing warning during `make -B RHP.o`.
- [ ] Full regression testing has not been completed after the security fixes.

## Verification

| Command | Result |
| ------- | ------ |
| `make -B RHP.o` | Completed with an existing `RHP.c` warning. |
| Full object checks for `HTTPcode.c`, `APRSCode.c`, `nodeapi.c` | Blocked by `Rigresource.h` include case mismatch. |
| `mailapi.c` object compile | Blocked by missing `dbghelp.h`. |
