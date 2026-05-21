# Changelog

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
