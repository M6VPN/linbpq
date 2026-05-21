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
- [x] Build blocker: Local quoted include references now match the actual file casing, including `rigresource.h`.

## Known Problems

- [ ] `nodeapi.c` object compile is blocked by `tncinfo.h:931` declaring `LRESULT CALLBACK PacWndProc(...)` without Linux-visible `LRESULT` / `CALLBACK` definitions.
- [ ] `HTTPcode.c` object compile is blocked by `tncinfo.h:931` and later Windows-only `CreateFile`, registry, and related symbols being visible in the Linux compile path.
- [ ] `APRSCode.c` object compile is blocked by `tncinfo.h:931` and `LPSOCKADDR` being undefined in `TCPConnect` / `GPSDConnect`.
- [ ] `mailapi.c` object compile is blocked by the existing missing `dbghelp.h` include.
- [ ] `RHP.c` still emits an existing warning during `make -B RHP.o`.
- [ ] Full regression testing has not been completed after the security fixes.

## Verification

| Command | Result |
| ------- | ------ |
| `make -B RHP.o` | Completed with an existing `RHP.c` warning. |
| Local quoted include case scan | No remaining mismatches against existing local filenames. |
| `make -B nodeapi.o` | Blocked by `tncinfo.h:931`: unknown type name `LRESULT`. |
| `make -B HTTPcode.o` | Blocked by `tncinfo.h:931` and Windows-only symbols including `GENERIC_READ`, `GENERIC_WRITE`, `OPEN_EXISTING`, and registry constants. |
| `make -B APRSCode.o` | Blocked by `tncinfo.h:931` and undefined `LPSOCKADDR`. |
| `mailapi.c` object compile | Blocked by missing `dbghelp.h`. |
