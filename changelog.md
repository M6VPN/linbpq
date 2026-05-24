# Changelog

## 2026-05-24

Source: current security fixes in this working tree.

### Security

- Fixed RHP send handling so malformed JSON string values and invalid `\u` escapes are rejected before decoding.
- Fixed WebMail B2 attachment parsing so malformed body/file lengths, filenames, and attachment boundaries are rejected before allocation or copying.
- Fixed WebMail select-list output format strings so literal HTML is not passed through malformed `sprintf` formats.
- Fixed Mail UI config updates so oversized fields, malformed `%XX` escapes, and excessive port counts are rejected before fixed-buffer writes or array indexing.
- Fixed HSMODEM, FreeDATA, and MULTIPSK connect command handling so oversized command text is rejected before writing fixed command buffers.
- Fixed Cmd downlink, MULTIPSK local command forwarding, and FreeDATA chat-call handling so oversized command text and calls are rejected before fixed-buffer writes.
- Fixed TNC emulator connected-status replies and serial TNC receive forwarding so oversized modem text is bounded before fixed-buffer writes.
- Fixed WebMail form directory recursion, config radio interlock conversion, and BBS housekeeping log/report output so oversized generated text is bounded before fixed-buffer writes.
- Fixed remaining Linux WebMail form and template path lookups so oversized generated paths are rejected before fixed-buffer writes.
- Fixed WebMail template date, sequence, form directory, and GPS substitutions so generated values are bounded before fixed-buffer writes.
- Fixed VARA and ARDOP local and connect command assembly so oversized command text and calls are rejected before fixed-buffer writes.
- Fixed ADIF log path, date, and comment formatting so oversized generated log records are rejected before fixed-buffer writes.
- Fixed Linux PG server execution so configured program names and generated paths are validated, argument lists are bounded, and `/bin/sh -c` is no longer used.
- Fixed YAPP upload handling so malformed filenames, sizes, generated paths, logs, and rejection replies are rejected or bounded before fixed-buffer writes.
- Fixed RMS Relay sync request handling so XML and request commands are bounded, allocation failures abort cleanly, and buffers are not used after free.
- Fixed SCS Tracker RADIO command and tracker reply handling so oversized node/TNC text is rejected or bounded before fixed-buffer writes.
- Fixed SCS Tracker multi-stream reply handling so oversized TNC response text is bounded before fixed-buffer writes.
- Fixed SCS Pactor RADIO command and CHECKLEVEL handling so oversized node/TNC text is rejected before fixed-buffer writes.
- Fixed manual KISS command response handling so oversized text and hex replies are rejected before fixed-buffer writes.
- Fixed POP3 LIST response handling so generated message-count replies are bounded before fixed-buffer writes.
- Fixed IP adapter status logging so configured adapter names are bounded before fixed-buffer writes.
- Fixed Linux Ethernet adapter handling so oversized adapter names are rejected before `ifreq` copies and status logging is bounded.
- Fixed FLDigi XML command assembly so string parameters are XML-escaped, integer commands use the integer helper, and generated RPC buffers are bounded before sending.
- Fixed FLDigi default modem handling so oversized configured modem names are rejected before fixed-buffer copies and generated KISS commands are bounded before sending.

### Verification

- `git diff --check` completed.
- `make clean` completed.
- `make nomqtt` completed and linked `linbpq`.
- Clean `nomqtt` build rebuilt `mqtt.o` with `-DNOMQTT`.
- `make -B BBSHTMLConfig.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` completed with remaining existing unrelated warnings; the `ProcessUIUpdate` format-overflow warnings are cleared.
- `make clean` completed before the final Mail UI config verification build.
- `make nomqtt` completed and linked `linbpq` after the Mail UI config fix.
- `make -B HSMODEM.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` completed with remaining existing unrelated warnings; the connect command format-overflow warning is cleared.
- `make -B FreeDATA.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` completed with remaining existing unrelated warnings; the connect command format-overflow warning is cleared.
- `make -B MULTIPSK.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` completed with remaining existing unrelated warnings; the missing-call and command-response warnings are cleared.
- `make clean` completed before the final TNC connect command verification build.
- `make nomqtt` completed and linked `linbpq` after the TNC connect command fix.
- `make -B Cmd.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` completed with remaining existing unrelated warnings; the downlink connect `Callstring` format-overflow warning is cleared.
- `make -B MULTIPSK.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` completed with remaining existing unrelated warnings; the `DIGITAL MODE` and generic command format-overflow warnings are cleared.
- `make -B FreeDATA.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` completed with remaining existing unrelated warnings; the chat-call response warning is cleared.
- `make clean` completed before the final downlink command verification build.
- `make nomqtt` completed and linked `linbpq` after the downlink command fix.
- `make -B TNCEmulators.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` completed with remaining existing unrelated warnings; the `STATUSPOLL` connected-status warning is cleared.
- `make -B SerialPort.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` completed with remaining existing unrelated warnings; the serial receive forwarding warning is cleared.
- `make clean` completed before the final TNC emulator and serial receive verification build.
- `make nomqtt` completed and linked `linbpq` after the TNC emulator and serial receive fix.
- `make -B WebMail.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` completed with remaining existing unrelated warnings; the form directory path warning is cleared.
- `make -B config.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` completed; the radio interlock conversion warning is cleared.
- `make -B Housekeeping.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` completed with remaining existing unrelated warnings; the log filename and totals report warnings are cleared.
- `make clean` completed before the final WebMail, config, and housekeeping verification build.
- `make nomqtt` completed and linked `linbpq` after the WebMail, config, and housekeeping fix.
- `make -B WebMail.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` completed with remaining existing unrelated warnings; the remaining Linux form and template path warnings are cleared.
- `make clean` completed before the final WebMail form path verification build.
- `make nomqtt` completed and linked `linbpq` after the remaining WebMail form path fix.
- `make -B WebMail.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` completed with remaining existing unrelated warnings; the template date, sequence, form directory, and GPS formatting warnings are cleared.
- `make clean` completed before the final WebMail template date verification build.
- `make nomqtt` completed and linked `linbpq` after the WebMail template date fix.
- `make -B VARA.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` completed with remaining existing unrelated warnings; the radio and connect command format-overflow and overlap warnings are cleared.
- `make -B ARDOP.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` completed with remaining existing unrelated warnings; the ARQ and packet connect command format-overflow warnings are cleared.
- `make clean` completed before the final VARA and ARDOP command verification build.
- `make nomqtt` completed and linked `linbpq` after the VARA and ARDOP command fix.
- `make -B adif.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` completed with remaining existing unrelated warnings; the ADIF date and comment format-overflow warnings are cleared.
- `make clean` completed before the final ADIF log verification build.
- `make nomqtt` completed and linked `linbpq` after the ADIF log bounds fix.
- `make -B BBSUtilities.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` completed with remaining existing unrelated warnings; the Linux PG server command path and shell command assembly warnings are cleared.
- `make clean` completed before the final Linux PG server execution verification build.
- `make nomqtt` completed and linked `linbpq` after the Linux PG server execution fix.
- `make -f makefile -B BBSUtilities.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` completed with remaining existing unrelated warnings; the YAPP upload rejection reply warnings are cleared.
- `make -f makefile clean` completed before the final YAPP upload header verification build.
- `make -f makefile nomqtt` completed and linked `linbpq` after the YAPP upload header fix.
- `make -f makefile -B BBSUtilities.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` completed with remaining existing unrelated warnings; the RMS Relay sync request format and use-after-free warnings are cleared.
- `make -f makefile clean` completed before the final RMS Relay sync request verification build.
- `make -f makefile nomqtt` completed and linked `linbpq` after the RMS Relay sync request fix.
- `make -f makefile -B SCSTracker.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` completed with remaining existing unrelated warnings; the SCS Tracker RADIO command and tracker reply overflow/overlap warnings are cleared.
- `make -f makefile clean` completed before the final SCS Tracker bounds verification build.
- `make -f makefile nomqtt` completed and linked `linbpq` after the SCS Tracker bounds fix.
- `make -f makefile -B SCSTrackeMulti.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` completed with remaining existing unrelated warnings; the multi-stream tracker reply overflow warning is cleared.
- `make -f makefile clean` completed before the final SCS Tracker multi-stream verification build.
- `make -f makefile nomqtt` completed and linked `linbpq` after the SCS Tracker multi-stream bounds fix.
- `make -f makefile -B SCSPactor.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` completed with remaining existing unrelated warnings; the SCS Pactor RADIO command and CHECKLEVEL overflow/overlap warnings are cleared.
- `make -f makefile clean` completed before the final SCS Pactor bounds verification build.
- `make -f makefile nomqtt` completed and linked `linbpq` after the SCS Pactor bounds fix.
- `make -f makefile -B kiss.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` completed with remaining existing unrelated warnings; the manual KISS command response overflow warning is cleared.
- `make -f makefile clean` completed before the final manual KISS response verification build.
- `make -f makefile nomqtt` completed and linked `linbpq` after the manual KISS response bounds fix.
- `make -f makefile -B MailTCP.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` completed with remaining existing unrelated warnings; the POP3 LIST response overflow warning is cleared.
- `make -f makefile clean` completed before the final POP3 LIST response verification build.
- `make -f makefile nomqtt` completed and linked `linbpq` after the POP3 LIST response bounds fix.
- `make -f makefile -B IPCode.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` completed with remaining existing unrelated warnings; the IP adapter status overflow warnings are cleared.
- `make -f makefile clean` completed before the final IP adapter status verification build.
- `make -f makefile nomqtt` completed and linked `linbpq` after the IP adapter status bounds fix.
- `make -f makefile -B linether.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` completed with remaining existing unrelated warnings; the Linux Ethernet adapter status overflow warning is cleared.
- `make -f makefile clean` completed before the final Linux Ethernet adapter verification build.
- `make -f makefile nomqtt` completed and linked `linbpq` after the Linux Ethernet adapter bounds fix.
- `make -f makefile -B FLDigi.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` completed with remaining existing unrelated warnings; the FLDigi XML command integer format warning is cleared.
- `make -f makefile clean` completed before the final FLDigi XML command verification build.
- `make -f makefile nomqtt` completed and linked `linbpq` after the FLDigi XML command bounds fix.
- `make -f makefile -B FLDigi.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` completed with remaining existing unrelated warnings; the FLDigi default modem command overflow warning is cleared.
- `make -f makefile clean` completed before the final FLDigi default modem verification build.
- `make -f makefile nomqtt` completed and linked `linbpq` after the FLDigi default modem bounds fix.

## 2026-05-22

Source: current WebMail fixes in this working tree.

### Security

- Fixed WebMail attachment downloads so invalid requested attachment numbers are rejected before indexing fixed attachment arrays.
- Fixed WebMail message uploads so excess multipart attachments are rejected before indexing fixed attachment arrays.
- Fixed WebMail template form submissions so excess multipart fields are rejected before indexing fixed key/value arrays.
- Fixed WebMail delete requests so missing message IDs return a not-found page instead of dereferencing a missing message record.
- Fixed WebMail reply and quote-original requests so message ownership is checked before message content is exposed.

### Verification

- `make -B WebMail.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` completed with existing unrelated warnings.
- `make -B WebMail.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` completed with remaining existing unrelated warnings.
- `make all` completed and relinked `linbpq`.
- `getcap ./linbpq` reported no file capabilities after relink.
- `./linbpq -h` exited cleanly and printed usage.
- `timeout 12s ./linbpq -c /tmp/linbpq-runtime-test/config -d /tmp/linbpq-runtime-test/data -l /tmp/linbpq-runtime-test/log` started with `bpq32.cfg.example`, initialized Telnet, Chat, and Mail, stayed running until timeout, and closed ports.
- `pgrep -af linbpq` found no running `linbpq` process after the timeout test.
- `make -B WebMail.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` completed with existing unrelated warnings after the WebMail reply ownership fix.
- `make -B WebMail.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` completed with remaining existing unrelated warnings after the WebMail reply ownership fix.
- `make all` completed and relinked `linbpq` after the WebMail reply ownership fix.
- `getcap ./linbpq` reported no file capabilities after relink.
- `./linbpq -h` exited cleanly and printed usage after the WebMail reply ownership fix.
- `timeout 12s ./linbpq -c /tmp/linbpq-runtime-test/config -d /tmp/linbpq-runtime-test/data -l /tmp/linbpq-runtime-test/log` started with `bpq32.cfg.example`, initialized Telnet, Chat, and Mail, stayed running until timeout, and closed ports after the WebMail reply ownership fix.
- `pgrep -af linbpq` found no running `linbpq` process after the timeout test.

## 2026-05-21

Sources: commits `75e083c`, `bfff949`, `66478d8`, `fbf82a0`, `1946167`, `4b31822`, `62b037b`, `92f6e91`, `207953b`, and `8bb38e5`, plus the current RHP receive bounds fix in this working tree.

### Security

- Fixed protected node API routes so read-only endpoints remain public while mutating and protected routes require a valid API key.
- Fixed local include casing so quoted includes match the actual `rigresource.h` filename on case-sensitive Linux filesystems.
- Fixed TriMode telnet debug logging so network/session text is logged with a literal format string.
- Fixed APRS debug logging so APRS-IS, RF, NMEA, AIS, and weather text is logged with a literal format string.
- Fixed Telnet outward-connect command parsing and signon assembly so oversized tokens are rejected before they can overflow fixed buffers.
- Fixed Telnet, relay, and CMS login logging so oversized remote username, password, and signon text is bounded before writing to stack log buffers.
- Fixed public node API port endpoints so invalid port numbers are rejected before indexing fixed port tables.
- Fixed BBS attachment download parsing so malformed or oversized stored attachment filenames are rejected before fixed-buffer copies.
- Fixed RHP websocket receive forwarding so packet data is JSON-escaped into sized buffers before websocket frames are built.

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
- `make -B BBSHTMLConfig.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` completed with existing unrelated format-overflow warnings after the BBS attachment filename bounds fix.
- `make -B BBSHTMLConfig.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security'` completed with remaining existing unrelated warnings after the BBS attachment filename bounds fix.
- `make all` completed and relinked `linbpq` after the BBS attachment filename bounds fix.
- `./linbpq -h` exited cleanly and printed usage after the BBS attachment filename bounds fix.
- `timeout 12s ./linbpq -c /tmp/linbpq-runtime-test/config -d /tmp/linbpq-runtime-test/data -l /tmp/linbpq-runtime-test/log` started with `bpq32.cfg.example`, initialized Telnet, Chat, and Mail, stayed running until timeout, and closed ports after the BBS attachment filename bounds fix.
- `pgrep -af linbpq` found no running `linbpq` process after the timeout test.
- `make -B RHP.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables'` completed with no warnings after the RHP receive bounds fix.
- `make -B RHP.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds'` completed with remaining non-security pointer-sign and unused warnings after the RHP receive bounds fix; the `RHPPoll` format-overflow warning is cleared.
- `make all` completed and relinked `linbpq` after the RHP receive bounds fix.
- `getcap ./linbpq || true` reported no file capabilities after relink.
- `./linbpq -h` exited cleanly and printed usage after the RHP receive bounds fix.
- `timeout 12s ./linbpq -c /tmp/linbpq-runtime-test/config -d /tmp/linbpq-runtime-test/data -l /tmp/linbpq-runtime-test/log` started with `bpq32.cfg.example`, initialized Telnet, Chat, and Mail, stayed running until timeout, and closed ports after the RHP receive bounds fix.
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
