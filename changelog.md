# Changelog

## 2026-05-21

Sources: commits `75e083c`, `bfff949`, `66478d8`, `fbf82a0`, `1946167`, `4b31822`, `62b037b`, and `92f6e91`, plus the current node API port bounds fix in this working tree.

### Security

- Fixed protected node API routes so read-only endpoints remain public while mutating and protected routes require a valid API key.
- Fixed local include casing so quoted includes match the actual `rigresource.h` filename on case-sensitive Linux filesystems.
- Fixed TriMode telnet debug logging so network/session text is logged with a literal format string.
- Fixed APRS debug logging so APRS-IS, RF, NMEA, AIS, and weather text is logged with a literal format string.
- Fixed Telnet outward-connect command parsing and signon assembly so oversized tokens are rejected before they can overflow fixed buffers.
- Fixed Telnet, relay, and CMS login logging so oversized remote username, password, and signon text is bounded before writing to stack log buffers.
- Fixed public node API port endpoints so invalid port numbers are rejected before indexing fixed port tables.

### Linux Build and Runtime

- Updated the GNU/Linux build workflow so `make all` compiles and links `linbpq` without running `sudo`.
- Added a `make setcap` target that prints the required root-only `setcap` command instead of running it.
- Added build artifact ignores for object, dependency, binary, and map outputs.
- Added `LOCATOR=NONE` to the local telnet-only example config so runtime smoke tests do not emit the missing locator warning.
- Verified `./linbpq -h`, `ldd ./linbpq`, and a temporary loopback daemon runtime smoke test.

### Verification

- `make -B nodeapi.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` completed.
- `make -B HTTPcode.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` completed with existing warnings.
- `make -B APRSCode.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` completed with existing warnings.
- `make -B mailapi.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` completed with existing warnings.
- `make -B TelnetV6.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` completed with remaining existing unrelated warnings; the outward-connect signon warnings are cleared.
- `make -B TelnetV6.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security'` completed with remaining existing unrelated warnings; the fixed login logging sites no longer emit format overflow warnings.
- `make -B nodeapi.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` completed after the node API port bounds fix.
- `make -B nodeapi.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security'` completed with remaining existing unrelated warnings; the fixed port parser no longer emits the assignment-as-condition warning.
- `make all` completed and linked `linbpq` after the node API port bounds fix.
- `./linbpq -h` exited cleanly and printed usage after the node API port bounds fix.
- `timeout 12s ./linbpq -c /tmp/linbpq-runtime-test/config -d /tmp/linbpq-runtime-test/data -l /tmp/linbpq-runtime-test/log` started with `bpq32.cfg.example`, initialized Telnet, Chat, and Mail, stayed running until timeout, and closed ports after the node API port bounds fix.
- `pgrep -af linbpq` found no running `linbpq` process after the timeout test.

## 2026-05-20

Source: commit `3d14e3d7d48da4ff4e800884a95f48e65e02036b` (`first set of security fixes`).

### Security

- Fixed HTTP session cookie handling so `BPQSessionCookie=N...` is trusted only when it maps to a live session.
- Fixed direct APRS message POST handling so remote requests require either local access or a valid session.
- Fixed APRS callsign handling so missing callsign input is rejected instead of being dereferenced.
- Fixed RHP websocket stream opens so they require an authenticated secure websocket session.
- Fixed rig and RHP websocket control paths so cookie presence alone is not treated as authentication.
- Replaced time-derived HTTP session keys with OS random bytes, with a fallback only if the OS random source fails.
- Replaced `rand()` API token generation in `nodeapi.c` and `mailapi.c` with OS random bytes.
- Added bounded copies for beacon destination, file, and text fields, and reject invalid beacon ports.

### Web UI

- Added `/m6vpn.css` and `/m6vpn-ui.js` references across node, APRS, mail, webmail, chat, rig, status, and driver pages.
- Removed many hardcoded `background` and `bgcolor` attributes so common styling can be handled by the shared UI assets.

### Documentation

- Added `README.md` with security fix notes, patch generation command, and verification notes.

### Verification

- `make -B RHP.o` completed, with an existing warning in `RHP.c`.
- Full object checks for `HTTPcode.c`, `APRSCode.c`, and `nodeapi.c` are blocked by the existing `Rigresource.h` include case mismatch.
- `mailapi.c` object compile is blocked by the existing missing `dbghelp.h` include.
