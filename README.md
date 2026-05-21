# LinBPQ Security Fix Notes

This copy includes minimal security fixes for confirmed high-impact issues found in the HTTP, API, APRS, and websocket handling paths.

## Fixed Issues

| Severity | Area | Fix |
| -------- | ---- | --- |
| Critical | HTTP session cookies | `BPQSessionCookie=N...` is only trusted when it resolves to a live session. |
| Critical | APRS message POST | Direct APRS message POST now requires local access or a valid session. Missing callsign input is rejected safely. |
| High     | RHP websocket | RHP stream open requests now require an authenticated secure websocket session. |
| High     | Rig/RHP websocket auth | Websocket control paths no longer treat cookie presence alone as authentication. |
| High     | Session and API tokens | Session keys and API tokens now use OS random bytes instead of time-derived values or `rand()`. |
| High     | Beacon form input | Beacon form writes use bounded copies for destination, file, and text fields, and reject invalid ports. |

## Patch Command

Generate a patch file for the security fixes with:

```bash
cd /home/dgm/Sync/Code/M6VPN/M6VPN-7/3rd/linbpq && git diff -- HTTPcode.c TelnetV6.c APRSCode.c RHP.c nodeapi.c mailapi.c README.md > linbpq-security-fixes.patch
```

## Verification Notes

- `make all` compiles and links `linbpq` on Linux without running `sudo`.
- `./linbpq -h` exits cleanly and prints usage.
- `ldd ./linbpq` resolves the required shared libraries.
- `getcap ./linbpq` reports the required packet/network capabilities on the current binary.
- `timeout 12s ./linbpq -c /tmp/linbpq-runtime-test/config -d /tmp/linbpq-runtime-test/data -l /tmp/linbpq-runtime-test/log` starts with a temporary internal loopback config, initializes port 1, stays running until timeout, and closes ports.
- `sudo -n setcap "CAP_NET_ADMIN=ep CAP_NET_RAW=ep CAP_NET_BIND_SERVICE=ep" ./linbpq` is blocked in this shell by password prompting.
