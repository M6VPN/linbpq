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
- [x] Linux build: `make all` compiles and links `linbpq` without invoking `sudo`.
- [x] Linux runtime smoke test: `./linbpq -h` exits cleanly and prints usage.

## Known Problems

- [ ] `RHP.c` still emits an existing warning during `make -B RHP.o`.
- [ ] Linux full build emits existing format and buffer-size warnings across several C files.
- [ ] Runtime packet/network capabilities require running the `setcap` command shown by `make setcap` as root.
- [ ] Full regression testing has not been completed after the security fixes.

## Verification

| Command | Result |
| ------- | ------ |
| `make -B RHP.o` | Completed with an existing `RHP.c` warning. |
| Local quoted include case scan | No remaining mismatches against existing local filenames. |
| `make -B nodeapi.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` | Completed. |
| `make -B HTTPcode.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` | Completed with existing warnings. |
| `make -B APRSCode.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` | Completed with existing warnings. |
| `make -B mailapi.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` | Completed with existing warnings. |
| `make all` | Completed and linked `linbpq` without running `sudo`. |
| `./linbpq -h` | Exited cleanly and printed usage. |
| `ldd ./linbpq` | Required shared libraries resolved. |
