# Security

This file tracks fixed security issues and known open problems in this LinBPQ tree.

## Fixed

- [x] Critical: HTTP session cookies are only accepted when `BPQSessionCookie=N...` resolves to a live session. ([3d14e3d])
- [x] Critical: APRS message POST requires local access or a valid session. ([3d14e3d])
- [x] Critical: APRS message POST rejects missing callsign input safely. ([3d14e3d])
- [x] High: RHP websocket stream open requests require an authenticated secure websocket session. ([3d14e3d])
- [x] High: Rig and RHP websocket control paths no longer treat cookie presence alone as authentication. ([3d14e3d])
- [x] High: HTTP session keys use OS random bytes instead of time-derived values. ([3d14e3d])
- [x] High: Node API tokens use OS random bytes instead of `rand()`. ([3d14e3d])
- [x] High: Mail API tokens use OS random bytes instead of `rand()`. ([3d14e3d])
- [x] High: Beacon form destination, file, and text writes use bounded copies. ([3d14e3d])
- [x] High: Beacon form handling rejects invalid ports before using the port slot. ([3d14e3d])
- [x] Build blocker: Local quoted include references now match the actual file casing, including `rigresource.h`. ([bfff949])
- [x] Linux build: `make all` compiles and links `linbpq` without invoking `sudo`. ([66478d8])
- [x] Linux runtime smoke test: `./linbpq -h` exits cleanly and prints usage. ([66478d8])
- [x] Linux daemon runtime smoke test: temporary loopback config starts, initializes port 1, stays running until timeout, and closes ports. ([66478d8], [fbf82a0])
- [x] High: RHP session send, close, and status requests reject handles not owned by the current websocket session. ([75e083c])
- [x] High: TriMode telnet debug logging no longer uses network/session text as a `printf` format string. ([1946167])
- [x] High: APRS debug logging no longer uses APRS-IS, RF, NMEA, AIS, or weather text as a `printf` format string. ([4b31822])
- [x] High: Telnet outward-connect command parsing and signon assembly reject oversized tokens instead of overflowing fixed buffers. ([62b037b])
- [x] High: Telnet, relay, and CMS login logging use bounded formatting for remote username, password, and signon text. ([92f6e91])
- [x] High: Public node API port endpoints reject invalid port numbers before indexing fixed port tables. ([207953b])
- [x] High: BBS attachment download parsing rejects malformed or oversized stored attachment filenames before copying into fixed buffers. ([8bb38e5])
- [x] High: RHP websocket receive forwarding escapes packet data into sized buffers before building JSON websocket frames. ([77a294a])
- [x] Medium: WebMail attachment downloads reject invalid attachment indexes before indexing fixed attachment arrays. ([54ee17a])
- [x] High: WebMail message uploads reject excess attachments before indexing fixed attachment arrays. ([c051946])
- [x] High: WebMail template form submissions reject excess multipart fields before indexing fixed key/value arrays. ([874f364])
- [x] Medium: WebMail delete requests for missing message IDs return safely instead of dereferencing a missing message record. ([a5cab13])
- [x] High: WebMail reply and quote-original requests require message ownership before exposing message content. ([f788b88])
- [x] High: RHP send handling rejects malformed JSON string values and invalid `\u` escapes before decoding. ([8f15e89])
- [x] High: WebMail B2 attachment parsing rejects malformed body/file lengths, filenames, and attachment boundaries before allocation or copying. ([8f15e89])
- [x] Medium: WebMail select-list output uses valid literal `sprintf` formats. ([8f15e89])
- [x] High: Mail UI config updates reject oversized fields, malformed `%XX` escapes, and excessive port counts before fixed-buffer writes or array indexing. ([6dc6df5])
- [x] High: HSMODEM, FreeDATA, and MULTIPSK connect command handling rejects oversized command text before writing fixed command buffers. ([aca8da7])
- [x] High: Cmd downlink, MULTIPSK local command forwarding, and FreeDATA chat-call handling reject oversized command text and calls before fixed-buffer writes. ([7ea04ad])
- [x] High: TNC emulator connected-status replies and serial TNC receive forwarding bound oversized modem text before fixed-buffer writes. ([1cfcd38])
- [x] High: WebMail form recursion, config radio conversion, and BBS housekeeping output bound oversized generated text before fixed-buffer writes. ([f13da93])
- [x] High: Remaining Linux WebMail form and template path lookups reject oversized generated paths before fixed-buffer writes. ([c4b5f8e])
- [x] High: WebMail template date, sequence, form directory, and GPS substitutions bound generated values before fixed-buffer writes. ([06e1fb5])
- [x] High: VARA and ARDOP local and connect command assembly reject oversized command text and calls before fixed-buffer writes. ([75ac804])
- [x] High: ADIF log path, date, and comment formatting reject oversized generated log records before fixed-buffer writes. ([839726c])
- [x] High: Linux PG server execution validates configured program names, bounds generated paths and argument lists, and avoids `/bin/sh -c`. ([0986ef9])
- [x] High: YAPP upload handling rejects malformed filenames and sizes before fixed-buffer copies and bounds generated paths, logs, and rejection replies. ([6246c85])
- [x] High: RMS Relay sync request handling bounds generated XML and request commands and avoids use-after-free on request buffers. ([38f91a1])
- [x] High: SCS Tracker RADIO command and tracker reply handling rejects or bounds oversized node/TNC text before fixed-buffer writes. ([3be7619])
- [x] High: SCS Tracker multi-stream reply handling bounds oversized TNC response text before fixed-buffer writes. ([4cf53bb])
- [x] High: SCS Pactor RADIO command and CHECKLEVEL handling rejects oversized node/TNC text before fixed-buffer writes. ([129a552])
- [x] High: Manual KISS command response handling rejects oversized text and hex replies before fixed-buffer writes. ([bc3d541])
- [x] High: POP3 LIST response handling bounds generated message-count replies before fixed-buffer writes. ([57edf61])
- [x] High: IP adapter status logging bounds configured adapter names before fixed-buffer writes. ([a30cbd3])
- [x] High: Linux Ethernet adapter handling rejects oversized adapter names before `ifreq` copies and bounds status logging. ([b391328])
- [x] High: FLDigi UI frame reply handling rejects malformed lengths before indexing packet data and bounds generated UI replies before sending. ([73a8e67])
- [x] High: FLDigi XML command assembly escapes string parameters, rejects wrong parameter types, and bounds generated RPC buffers before sending. ([8969235])
- [x] High: FLDigi default modem handling rejects oversized configured modem names before fixed-buffer copies and bounds generated KISS commands before sending. ([344c685])

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
| `make -B adif.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` after the ADIF log bounds fix | Completed with remaining existing unrelated warnings; the ADIF date and comment format-overflow warnings are cleared. |
| `make clean` after the ADIF log bounds fix | Completed. |
| `make nomqtt` after the ADIF log bounds fix | Completed and linked `linbpq`. |
| `make -B BBSUtilities.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` after the Linux PG server execution fix | Completed with remaining existing unrelated warnings; the PG server command path and shell command assembly warnings are cleared. |
| `make clean` after the Linux PG server execution fix | Completed. |
| `make nomqtt` after the Linux PG server execution fix | Completed and linked `linbpq`. |
| `make -f makefile -B BBSUtilities.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` after the YAPP upload header fix | Completed with remaining existing unrelated warnings; the YAPP upload rejection reply warnings are cleared. |
| `make -f makefile clean` after the YAPP upload header fix | Completed. |
| `make -f makefile nomqtt` after the YAPP upload header fix | Completed and linked `linbpq`. |
| `make -f makefile -B BBSUtilities.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` after the RMS Relay sync request fix | Completed with remaining existing unrelated warnings; the request format and use-after-free warnings are cleared. |
| `make -f makefile clean` after the RMS Relay sync request fix | Completed. |
| `make -f makefile nomqtt` after the RMS Relay sync request fix | Completed and linked `linbpq`. |
| `make -f makefile -B SCSTracker.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` after the SCS Tracker bounds fix | Completed with remaining existing unrelated warnings; the RADIO command and tracker reply overflow/overlap warnings are cleared. |
| `make -f makefile clean` after the SCS Tracker bounds fix | Completed. |
| `make -f makefile nomqtt` after the SCS Tracker bounds fix | Completed and linked `linbpq`. |
| `make -f makefile -B SCSTrackeMulti.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` after the SCS Tracker multi-stream bounds fix | Completed with remaining existing unrelated warnings; the tracker reply overflow warning is cleared. |
| `make -f makefile clean` after the SCS Tracker multi-stream bounds fix | Completed. |
| `make -f makefile nomqtt` after the SCS Tracker multi-stream bounds fix | Completed and linked `linbpq`. |
| `make -f makefile -B SCSPactor.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` after the SCS Pactor bounds fix | Completed with remaining existing unrelated warnings; the RADIO command and CHECKLEVEL overflow/overlap warnings are cleared. |
| `make -f makefile clean` after the SCS Pactor bounds fix | Completed. |
| `make -f makefile nomqtt` after the SCS Pactor bounds fix | Completed and linked `linbpq`. |
| `make -f makefile -B kiss.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` after the manual KISS response bounds fix | Completed with remaining existing unrelated warnings; the manual response overflow warning is cleared. |
| `make -f makefile clean` after the manual KISS response bounds fix | Completed. |
| `make -f makefile nomqtt` after the manual KISS response bounds fix | Completed and linked `linbpq`. |
| `make -f makefile -B MailTCP.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` after the POP3 LIST response bounds fix | Completed with remaining existing unrelated warnings; the POP3 LIST response overflow warning is cleared. |
| `make -f makefile clean` after the POP3 LIST response bounds fix | Completed. |
| `make -f makefile nomqtt` after the POP3 LIST response bounds fix | Completed and linked `linbpq`. |
| `make -f makefile -B IPCode.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` after the IP adapter status bounds fix | Completed with remaining existing unrelated warnings; the IP adapter status overflow warnings are cleared. |
| `make -f makefile clean` after the IP adapter status bounds fix | Completed. |
| `make -f makefile nomqtt` after the IP adapter status bounds fix | Completed and linked `linbpq`. |
| `make -f makefile -B linether.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` after the Linux Ethernet adapter bounds fix | Completed with remaining existing unrelated warnings; the adapter status overflow warning is cleared. |
| `make -f makefile clean` after the Linux Ethernet adapter bounds fix | Completed. |
| `make -f makefile nomqtt` after the Linux Ethernet adapter bounds fix | Completed and linked `linbpq`. |
| `make -f makefile -B FLDigi.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` after the FLDigi UI frame reply bounds fix | Completed with remaining existing unrelated warnings; the UI frame reply overflow warning is cleared. |
| `make -f makefile clean` after the FLDigi UI frame reply bounds fix | Completed. |
| `make -f makefile nomqtt` after the FLDigi UI frame reply bounds fix | Completed and linked `linbpq`. |
| `make -f makefile -B FLDigi.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` after the FLDigi XML command bounds fix | Completed with remaining existing unrelated warnings; the XML command integer format warning is cleared. |
| `make -f makefile clean` after the FLDigi XML command bounds fix | Completed. |
| `make -f makefile nomqtt` after the FLDigi XML command bounds fix | Completed and linked `linbpq`. |
| `make -f makefile -B FLDigi.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` after the FLDigi default modem bounds fix | Completed with remaining existing unrelated warnings; the default modem command overflow warning is cleared. |
| `make -f makefile clean` after the FLDigi default modem bounds fix | Completed. |
| `make -f makefile nomqtt` after the FLDigi default modem bounds fix | Completed and linked `linbpq`. |

[73a8e67]: https://github.com/M6VPN/linbpq/commit/73a8e67edf2fba824df514d74c5fb4b45f42a5f4
[8969235]: https://github.com/M6VPN/linbpq/commit/896923589e5d572d6940e51b5464c4a3e5189bad
[344c685]: https://github.com/M6VPN/linbpq/commit/344c685d5b6f74cfaca26f366d387aec6d5a279d
[b391328]: https://github.com/M6VPN/linbpq/commit/b39132848c8e009a9660e1510ca844c3e9b8a026
[a30cbd3]: https://github.com/M6VPN/linbpq/commit/a30cbd3101867692436824a9b7ff02de9234ea96
[57edf61]: https://github.com/M6VPN/linbpq/commit/57edf61cc5eb2498d6f127a152d78f676dc30535
[bc3d541]: https://github.com/M6VPN/linbpq/commit/bc3d5410cf0c3398bcbb5ed61ddb9610d7d08daf
[129a552]: https://github.com/M6VPN/linbpq/commit/129a5521e28d0c82367eb1e822c3aad790c013f6
[4cf53bb]: https://github.com/M6VPN/linbpq/commit/4cf53bb96f11fd4140c751048fb49a89f5cbe8a7
[3be7619]: https://github.com/M6VPN/linbpq/commit/3be76198feaa7f3c30bea2f4ceeeeda80ddce11c
[38f91a1]: https://github.com/M6VPN/linbpq/commit/38f91a1debe7add8b81ad03fd6ffc817d7671b14
[6246c85]: https://github.com/M6VPN/linbpq/commit/6246c858ca8c8d3d32205f4ce67c1b325d611f90
[0986ef9]: https://github.com/M6VPN/linbpq/commit/0986ef9873e3389aca7131e6179a0499a1687beb
[839726c]: https://github.com/M6VPN/linbpq/commit/839726c4388dbf824957d73e56a099261980c47a
[75ac804]: https://github.com/M6VPN/linbpq/commit/75ac8040f48a1b279839f95c0da16cf22a160221
[06e1fb5]: https://github.com/M6VPN/linbpq/commit/06e1fb554e8292efd6c85b1b78e0cb01b88677cf
[c4b5f8e]: https://github.com/M6VPN/linbpq/commit/c4b5f8ee8503f2d33bada85458ef7369edba70b3
[f13da93]: https://github.com/M6VPN/linbpq/commit/f13da93498026fe124acc1071fb5865f5259fdf9
[1cfcd38]: https://github.com/M6VPN/linbpq/commit/1cfcd38bbfc8007f201e48a446bc24fb1fd74c9b
[7ea04ad]: https://github.com/M6VPN/linbpq/commit/7ea04ad00d4dba8d456a823bcbec5093c4df07c5
[aca8da7]: https://github.com/M6VPN/linbpq/commit/aca8da76f5043cc54c9f24d1b8cff5f99f540c0f
[6dc6df5]: https://github.com/M6VPN/linbpq/commit/6dc6df51ff70dc82f272cc6b720b01c7743ead9b
[8f15e89]: https://github.com/M6VPN/linbpq/commit/8f15e89e659c42b23286c0355b8985dc11bc3481
[f788b88]: https://github.com/M6VPN/linbpq/commit/f788b88e23cfc19b8d0ba4ea7cfc4d78ddfdd0a7
[a5cab13]: https://github.com/M6VPN/linbpq/commit/a5cab1399c03c7610663e34ced4ee93755612000
[874f364]: https://github.com/M6VPN/linbpq/commit/874f364e005e2f662798e65b0ff35508464c770a
[c051946]: https://github.com/M6VPN/linbpq/commit/c05194695c0989fbb19e722a7f81ebee431d745d
[54ee17a]: https://github.com/M6VPN/linbpq/commit/54ee17aaf72f8b9d83164afe1639a2a4e4d3e200
[77a294a]: https://github.com/M6VPN/linbpq/commit/77a294a83a1580fc1b835f6516888585fc81864c
[8bb38e5]: https://github.com/M6VPN/linbpq/commit/8bb38e526230c30d79b7869cbf55c24003dc3993
[207953b]: https://github.com/M6VPN/linbpq/commit/207953b8c82805652f5d1de9e12921d3756016e2
[92f6e91]: https://github.com/M6VPN/linbpq/commit/92f6e91784345e41203aaf8d22f54b24c21162fd
[62b037b]: https://github.com/M6VPN/linbpq/commit/62b037be0f2f184fa19ba8de2c30b5deb72ffae0
[4b31822]: https://github.com/M6VPN/linbpq/commit/4b31822ebd71c1f59094b5f4ce9881c85c1ec444
[1946167]: https://github.com/M6VPN/linbpq/commit/1946167fd4cb6832945d2f4375340680b08c2f3c
[fbf82a0]: https://github.com/M6VPN/linbpq/commit/fbf82a092df6e3c49c34ebdf3fffe0dcdfab296d
[66478d8]: https://github.com/M6VPN/linbpq/commit/66478d8d57ef5e2a1a5bc63d81ad48edc6320c7a
[bfff949]: https://github.com/M6VPN/linbpq/commit/bfff9497d1c0db54fd6f3e3b14189e7e74d148af
[75e083c]: https://github.com/M6VPN/linbpq/commit/75e083c509483883986dd0ce5f5cf316400fe67f
[3d14e3d]: https://github.com/M6VPN/linbpq/commit/3d14e3d7d48da4ff4e800884a95f48e65e02036b
