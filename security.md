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
- [x] Linux daemon runtime smoke test: temporary loopback config starts, initializes port 1, stays running until timeout, and closes ports.
- [x] High: TriMode telnet debug logging no longer uses network/session text as a `printf` format string.
- [x] High: APRS debug logging no longer uses APRS-IS, RF, NMEA, AIS, or weather text as a `printf` format string.

## Known Problems

- [ ] `RHP.c` still emits an existing warning during `make -B RHP.o`.
- [ ] Linux full build emits existing format and buffer-size warnings across several C files.
- [ ] `sudo setcap` testing is blocked in this shell by password prompting; `make all` relinks `linbpq` and leaves file capabilities unset.
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
| `getcap ./linbpq` | No capabilities are set after the latest `make all` relink. |
| `sudo -n setcap "CAP_NET_ADMIN=ep CAP_NET_RAW=ep CAP_NET_BIND_SERVICE=ep" ./linbpq` | Blocked by `sudo: a password is required`. |
| `timeout 12s ./linbpq -c /tmp/linbpq-runtime-test/config -d /tmp/linbpq-runtime-test/data -l /tmp/linbpq-runtime-test/log` | Started with a temporary internal loopback config, initialized port 1, stayed running until timeout, and closed ports. |
| `pgrep -af linbpq` | No running `linbpq` process remained after the timeout test. |
| `make -B TelnetV6.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` | Completed with existing unrelated warnings. |
| `make -B APRSCode.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` | Completed with existing unrelated warnings. |
| `timeout 12s ./linbpq -c /tmp/linbpq-runtime-test/config -d /tmp/linbpq-runtime-test/data -l /tmp/linbpq-runtime-test/log` using `bpq32.cfg.example` | Started Telnet, Chat, and Mail, stayed running until timeout, and closed ports. |
