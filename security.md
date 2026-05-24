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
- [x] High: Telnet outward-connect command parsing and signon assembly reject oversized tokens instead of overflowing fixed buffers.
- [x] High: Telnet, relay, and CMS login logging use bounded formatting for remote username, password, and signon text.
- [x] High: Public node API port endpoints reject invalid port numbers before indexing fixed port tables.
- [x] High: BBS attachment download parsing rejects malformed or oversized stored attachment filenames before copying into fixed buffers.
- [x] High: RHP websocket receive forwarding escapes packet data into sized buffers before building JSON websocket frames.
- [x] Medium: WebMail attachment downloads reject invalid attachment indexes before indexing fixed attachment arrays.
- [x] High: WebMail message uploads reject excess attachments before indexing fixed attachment arrays.
- [x] High: WebMail template form submissions reject excess multipart fields before indexing fixed key/value arrays.
- [x] Medium: WebMail delete requests for missing message IDs return safely instead of dereferencing a missing message record.
- [x] High: WebMail reply and quote-original requests require message ownership before exposing message content.
- [x] High: RHP send handling rejects malformed JSON string values and invalid `\u` escapes before decoding.
- [x] High: WebMail B2 attachment parsing rejects malformed body/file lengths, filenames, and attachment boundaries before allocation or copying.
- [x] Medium: WebMail select-list output uses valid literal `sprintf` formats.
- [x] High: Mail UI config updates reject oversized fields, malformed `%XX` escapes, and excessive port counts before fixed-buffer writes or array indexing.
- [x] High: HSMODEM, FreeDATA, and MULTIPSK connect command handling rejects oversized command text before writing fixed command buffers.
- [x] High: Cmd downlink, MULTIPSK local command forwarding, and FreeDATA chat-call handling reject oversized command text and calls before fixed-buffer writes.
- [x] High: TNC emulator connected-status replies and serial TNC receive forwarding bound oversized modem text before fixed-buffer writes.
- [x] High: WebMail form recursion, config radio conversion, and BBS housekeeping output bound oversized generated text before fixed-buffer writes.
- [x] High: Remaining Linux WebMail form and template path lookups reject oversized generated paths before fixed-buffer writes.
- [x] High: WebMail template date, sequence, form directory, and GPS substitutions bound generated values before fixed-buffer writes.
- [x] High: VARA and ARDOP local and connect command assembly reject oversized command text and calls before fixed-buffer writes.

## Known Problems

- [ ] Linux full build emits existing format and buffer-size warnings across several C files.
- [ ] `make all` relinks `linbpq` and leaves file capabilities unset; `sudo setcap` was not run in the latest no-sudo audit pass.
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
| `make -B TelnetV6.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` | Completed with remaining existing unrelated warnings; the outward-connect signon warnings are cleared. |
| `make -B APRSCode.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` | Completed with existing unrelated warnings. |
| `timeout 12s ./linbpq -c /tmp/linbpq-runtime-test/config -d /tmp/linbpq-runtime-test/data -l /tmp/linbpq-runtime-test/log` using `bpq32.cfg.example` | Started Telnet, Chat, and Mail, stayed running until timeout, and closed ports. |
| `make -B TelnetV6.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` after the Telnet logging fix | Completed with remaining existing unrelated warnings. |
| `make -B TelnetV6.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security'` after the Telnet logging fix | Completed with remaining existing unrelated warnings; the fixed login logging sites no longer emit format overflow warnings. |
| `make all` after the Telnet logging fix | Completed and relinked `linbpq`; file capabilities may be unset after relink. |
| `./linbpq -h` after the Telnet logging fix | Exited cleanly and printed usage. |
| `timeout 12s ./linbpq -c /tmp/linbpq-runtime-test/config -d /tmp/linbpq-runtime-test/data -l /tmp/linbpq-runtime-test/log` after the Telnet logging fix | Started with the temporary runtime config, stayed running until timeout, and closed ports. |
| `pgrep -af linbpq` after the Telnet logging fix | No running `linbpq` process remained after the timeout test. |
| `make -B nodeapi.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` after the node API port bounds fix | Completed. |
| `make -B nodeapi.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security'` after the node API port bounds fix | Completed with remaining existing unrelated warnings; the fixed port parser no longer emits the assignment-as-condition warning. |
| `make all` after the node API port bounds fix | Completed and relinked `linbpq`; file capabilities may be unset after relink. |
| `./linbpq -h` after the node API port bounds fix | Exited cleanly and printed usage. |
| `timeout 12s ./linbpq -c /tmp/linbpq-runtime-test/config -d /tmp/linbpq-runtime-test/data -l /tmp/linbpq-runtime-test/log` after the node API port bounds fix | Started with `bpq32.cfg.example`, initialized Telnet, Chat, and Mail, stayed running until timeout, and closed ports. |
| `pgrep -af linbpq` after the node API port bounds fix | No running `linbpq` process remained after the timeout test. |
| `make -B BBSHTMLConfig.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` after the BBS attachment filename bounds fix | Completed with existing unrelated format-overflow warnings in `ProcessUIUpdate`. |
| `make -B BBSHTMLConfig.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security'` after the BBS attachment filename bounds fix | Completed with remaining existing unrelated warnings. |
| `make all` after the BBS attachment filename bounds fix | Completed and relinked `linbpq`; file capabilities may be unset after relink. |
| `./linbpq -h` after the BBS attachment filename bounds fix | Exited cleanly and printed usage. |
| `timeout 12s ./linbpq -c /tmp/linbpq-runtime-test/config -d /tmp/linbpq-runtime-test/data -l /tmp/linbpq-runtime-test/log` after the BBS attachment filename bounds fix | Started with `bpq32.cfg.example`, initialized Telnet, Chat, and Mail, stayed running until timeout, and closed ports. |
| `pgrep -af linbpq` after the BBS attachment filename bounds fix | No running `linbpq` process remained after the timeout test. |
| `make -B RHP.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` before the RHP receive bounds fix | Completed with a format-overflow warning in `RHPPoll`. |
| `make -B RHP.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` after the RHP receive bounds fix | Completed with no warnings. |
| `make -B RHP.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` after the RHP receive bounds fix | Completed with remaining non-security pointer-sign and unused warnings; the `RHPPoll` format-overflow warning is cleared. |
| `make all` after the RHP receive bounds fix | Completed and relinked `linbpq`; file capabilities may be unset after relink. |
| `getcap ./linbpq || true` after the RHP receive bounds fix | No capabilities are set after relink. |
| `./linbpq -h` after the RHP receive bounds fix | Exited cleanly and printed usage. |
| `timeout 12s ./linbpq -c /tmp/linbpq-runtime-test/config -d /tmp/linbpq-runtime-test/data -l /tmp/linbpq-runtime-test/log` after the RHP receive bounds fix | Started with `bpq32.cfg.example`, initialized Telnet, Chat, and Mail, stayed running until timeout, and closed ports. |
| `pgrep -af linbpq` after the RHP receive bounds fix | No running `linbpq` process remained after the timeout test. |
| `make -B WebMail.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` after the WebMail attachment index fix | Completed with existing unrelated warnings. |
| `make -B WebMail.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` after the WebMail attachment index fix | Completed with remaining existing unrelated warnings. |
| `make all` after the WebMail attachment index fix | Completed and relinked `linbpq`; file capabilities may be unset after relink. |
| `getcap ./linbpq` after the WebMail attachment index fix | No capabilities are set after relink. |
| `./linbpq -h` after the WebMail attachment index fix | Exited cleanly and printed usage. |
| `timeout 12s ./linbpq -c /tmp/linbpq-runtime-test/config -d /tmp/linbpq-runtime-test/data -l /tmp/linbpq-runtime-test/log` after the WebMail attachment index fix | Started with `bpq32.cfg.example`, initialized Telnet, Chat, and Mail, stayed running until timeout, and closed ports. |
| `pgrep -af linbpq` after the WebMail attachment index fix | No running `linbpq` process remained after the timeout test. |
| `make -B WebMail.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` after the WebMail upload attachment count fix | Completed with existing unrelated warnings. |
| `make -B WebMail.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` after the WebMail upload attachment count fix | Completed with remaining existing unrelated warnings. |
| `make all` after the WebMail upload attachment count fix | Completed and relinked `linbpq`; file capabilities may be unset after relink. |
| `getcap ./linbpq` after the WebMail upload attachment count fix | No capabilities are set after relink. |
| `./linbpq -h` after the WebMail upload attachment count fix | Exited cleanly and printed usage. |
| `timeout 12s ./linbpq -c /tmp/linbpq-runtime-test/config -d /tmp/linbpq-runtime-test/data -l /tmp/linbpq-runtime-test/log` after the WebMail upload attachment count fix | Started with `bpq32.cfg.example`, initialized Telnet, Chat, and Mail, stayed running until timeout, and closed ports. |
| `pgrep -af linbpq` after the WebMail upload attachment count fix | No running `linbpq` process remained after the timeout test. |
| `make -B WebMail.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` after the WebMail form-field count fix | Completed with existing unrelated warnings. |
| `make -B WebMail.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` after the WebMail form-field count fix | Completed with remaining existing unrelated warnings. |
| `make all` after the WebMail form-field count fix | Completed and relinked `linbpq`; file capabilities may be unset after relink. |
| `getcap ./linbpq` after the WebMail form-field count fix | No capabilities are set after relink. |
| `./linbpq -h` after the WebMail form-field count fix | Exited cleanly and printed usage. |
| `timeout 12s ./linbpq -c /tmp/linbpq-runtime-test/config -d /tmp/linbpq-runtime-test/data -l /tmp/linbpq-runtime-test/log` after the WebMail form-field count fix | Started with `bpq32.cfg.example`, initialized Telnet, Chat, and Mail, stayed running until timeout, and closed ports. |
| `pgrep -af linbpq` after the WebMail form-field count fix | No running `linbpq` process remained after the timeout test. |
| `make -B WebMail.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` after the WebMail missing-message delete fix | Completed with existing unrelated warnings. |
| `make -B WebMail.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` after the WebMail missing-message delete fix | Completed with remaining existing unrelated warnings. |
| `make all` after the WebMail missing-message delete fix | Completed and relinked `linbpq`; file capabilities may be unset after relink. |
| `getcap ./linbpq` after the WebMail missing-message delete fix | No capabilities are set after relink. |
| `./linbpq -h` after the WebMail missing-message delete fix | Exited cleanly and printed usage. |
| `timeout 12s ./linbpq -c /tmp/linbpq-runtime-test/config -d /tmp/linbpq-runtime-test/data -l /tmp/linbpq-runtime-test/log` after the WebMail missing-message delete fix | Started with `bpq32.cfg.example`, initialized Telnet, Chat, and Mail, stayed running until timeout, and closed ports. |
| `pgrep -af linbpq` after the WebMail missing-message delete fix | No running `linbpq` process remained after the timeout test. |
| `make -B WebMail.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` after the WebMail reply ownership fix | Completed with existing unrelated warnings. |
| `make -B WebMail.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` after the WebMail reply ownership fix | Completed with remaining existing unrelated warnings. |
| `make all` after the WebMail reply ownership fix | Completed and relinked `linbpq`; file capabilities may be unset after relink. |
| `getcap ./linbpq` after the WebMail reply ownership fix | No capabilities are set after relink. |
| `./linbpq -h` after the WebMail reply ownership fix | Exited cleanly and printed usage. |
| `timeout 12s ./linbpq -c /tmp/linbpq-runtime-test/config -d /tmp/linbpq-runtime-test/data -l /tmp/linbpq-runtime-test/log` after the WebMail reply ownership fix | Started with `bpq32.cfg.example`, initialized Telnet, Chat, and Mail, stayed running until timeout, and closed ports. |
| `pgrep -af linbpq` after the WebMail reply ownership fix | No running `linbpq` process remained after the timeout test. |
| `git diff --check` after the RHP and WebMail parsing fixes | Completed. |
| `make clean` after the RHP and WebMail parsing fixes | Completed. |
| `make nomqtt` after the RHP and WebMail parsing fixes | Completed and linked `linbpq`; `mqtt.o` was rebuilt with `-DNOMQTT`. |
| `make -B BBSHTMLConfig.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` after the Mail UI config fix | Completed with remaining existing unrelated warnings; the `ProcessUIUpdate` format-overflow warnings are cleared. |
| `make clean` after the Mail UI config fix | Completed. |
| `make nomqtt` after the Mail UI config fix | Completed and linked `linbpq`. |
| `make -B HSMODEM.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` after the TNC connect command fix | Completed with remaining existing unrelated warnings; the connect command format-overflow warning is cleared. |
| `make -B FreeDATA.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` after the TNC connect command fix | Completed with remaining existing unrelated warnings; the connect command format-overflow warning is cleared. |
| `make -B MULTIPSK.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` after the TNC connect command fix | Completed with remaining existing unrelated warnings; the missing-call and command-response warnings are cleared. |
| `make clean` after the TNC connect command fix | Completed. |
| `make nomqtt` after the TNC connect command fix | Completed and linked `linbpq`. |
| `make -B Cmd.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` after the downlink command bounds fix | Completed with remaining existing unrelated warnings; the downlink connect `Callstring` format-overflow warning is cleared. |
| `make -B MULTIPSK.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` after the local command bounds fix | Completed with remaining existing unrelated warnings; the `DIGITAL MODE` and generic command format-overflow warnings are cleared. |
| `make -B FreeDATA.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` after the chat-call bounds fix | Completed with remaining existing unrelated warnings; the chat-call response warning is cleared. |
| `make clean` after the downlink command bounds fix | Completed. |
| `make nomqtt` after the downlink command bounds fix | Completed and linked `linbpq`. |
| `make -B TNCEmulators.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` after the emulator status bounds fix | Completed with remaining existing unrelated warnings; the `STATUSPOLL` connected-status warning is cleared. |
| `make -B SerialPort.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` after the serial receive bounds fix | Completed with remaining existing unrelated warnings; the serial receive forwarding warning is cleared. |
| `make clean` after the TNC emulator and serial receive fix | Completed. |
| `make nomqtt` after the TNC emulator and serial receive fix | Completed and linked `linbpq`. |
| `make -B WebMail.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` after the WebMail form path bounds fix | Completed with remaining existing unrelated warnings; the form directory path warning is cleared. |
| `make -B config.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` after the radio conversion bounds fix | Completed; the radio interlock conversion warning is cleared. |
| `make -B Housekeeping.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` after the housekeeping output bounds fix | Completed with remaining existing unrelated warnings; the log filename and totals report warnings are cleared. |
| `make clean` after the WebMail, config, and housekeeping fix | Completed. |
| `make nomqtt` after the WebMail, config, and housekeeping fix | Completed and linked `linbpq`. |
| `make -B WebMail.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` after the remaining WebMail form path bounds fix | Completed with remaining existing unrelated warnings; the remaining Linux form and template path warnings are cleared. |
| `make clean` after the remaining WebMail form path fix | Completed. |
| `make nomqtt` after the remaining WebMail form path fix | Completed and linked `linbpq`. |
| `make -B WebMail.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` after the WebMail template date bounds fix | Completed with remaining existing unrelated warnings; the template date, sequence, form directory, and GPS formatting warnings are cleared. |
| `make clean` after the WebMail template date fix | Completed. |
| `make nomqtt` after the WebMail template date fix | Completed and linked `linbpq`. |
| `make -B VARA.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` after the VARA command bounds fix | Completed with remaining existing unrelated warnings; the radio and connect command format-overflow and overlap warnings are cleared. |
| `make -B ARDOP.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` after the ARDOP command bounds fix | Completed with remaining existing unrelated warnings; the ARQ and packet connect command format-overflow warnings are cleared. |
| `make clean` after the VARA and ARDOP command fix | Completed. |
| `make nomqtt` after the VARA and ARDOP command fix | Completed and linked `linbpq`. |
