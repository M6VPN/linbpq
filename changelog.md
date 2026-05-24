# Changelog

## 2026-05-24

Sources: commits listed on each fixed security line.

### Security

- Fixed MULTIPSK and MULTIPSK64 data discard debug logging so TNC text is logged with a literal format string. ([8c62e11])
- Fixed MULTIPSK RADIO command rewriting and INUSE replies so oversized generated text is rejected or bounded before fixed-buffer writes. ([a6872dd])
- Fixed MULTIPSK and MULTIPSK64 default-mode and init-script handling so oversized config text cannot overflow fixed command or script buffers. ([4bc6c23])
- Fixed MULTIPSK64 local command handling so RADIO, MODE, connect, generic command, and internal command replies reject oversized text before fixed-buffer writes. ([5d0cc10])
- Fixed RHP send handling so malformed JSON string values and invalid `\u` escapes are rejected before decoding. ([8f15e89])
- Fixed WebMail B2 attachment parsing so malformed body/file lengths, filenames, and attachment boundaries are rejected before allocation or copying. ([8f15e89])
- Fixed WebMail select-list output format strings so literal HTML is not passed through malformed `sprintf` formats. ([8f15e89])
- Fixed Mail UI config updates so oversized fields, malformed `%XX` escapes, and excessive port counts are rejected before fixed-buffer writes or array indexing. ([6dc6df5])
- Fixed HSMODEM, FreeDATA, and MULTIPSK connect command handling so oversized command text is rejected before writing fixed command buffers. ([aca8da7])
- Fixed Cmd downlink, MULTIPSK local command forwarding, and FreeDATA chat-call handling so oversized command text and calls are rejected before fixed-buffer writes. ([7ea04ad])
- Fixed TNC emulator connected-status replies and serial TNC receive forwarding so oversized modem text is bounded before fixed-buffer writes. ([1cfcd38])
- Fixed WebMail form directory recursion, config radio interlock conversion, and BBS housekeeping log/report output so oversized generated text is bounded before fixed-buffer writes. ([f13da93])
- Fixed remaining Linux WebMail form and template path lookups so oversized generated paths are rejected before fixed-buffer writes. ([c4b5f8e])
- Fixed WebMail template date, sequence, form directory, and GPS substitutions so generated values are bounded before fixed-buffer writes. ([06e1fb5])
- Fixed VARA and ARDOP local and connect command assembly so oversized command text and calls are rejected before fixed-buffer writes. ([75ac804])
- Fixed ADIF log path, date, and comment formatting so oversized generated log records are rejected before fixed-buffer writes. ([839726c])
- Fixed Linux PG server execution so configured program names and generated paths are validated, argument lists are bounded, and `/bin/sh -c` is no longer used. ([0986ef9])
- Fixed YAPP upload handling so malformed filenames, sizes, generated paths, logs, and rejection replies are rejected or bounded before fixed-buffer writes. ([6246c85])
- Fixed RMS Relay sync request handling so XML and request commands are bounded, allocation failures abort cleanly, and buffers are not used after free. ([38f91a1])
- Fixed SCS Tracker RADIO command and tracker reply handling so oversized node/TNC text is rejected or bounded before fixed-buffer writes. ([3be7619])
- Fixed SCS Tracker multi-stream reply handling so oversized TNC response text is bounded before fixed-buffer writes. ([4cf53bb])
- Fixed SCS Pactor serial receive debug logging so TNC text is logged with a literal format string. ([e96206a])
- Fixed SCS Pactor RADIO command and CHECKLEVEL handling so oversized node/TNC text is rejected before fixed-buffer writes. ([129a552])
- Fixed manual KISS command response handling so oversized text and hex replies are rejected before fixed-buffer writes. ([bc3d541])
- Fixed POP3 LIST response handling so generated message-count replies are bounded before fixed-buffer writes. ([57edf61])
- Fixed IP adapter status logging so configured adapter names are bounded before fixed-buffer writes. ([a30cbd3])
- Fixed Linux Ethernet adapter handling so oversized adapter names are rejected before `ifreq` copies and status logging is bounded. ([b391328])
- Fixed FLDigi UI frame reply handling so malformed lengths are rejected before indexing packet data and generated UI replies are bounded before sending. ([73a8e67])
- Fixed FLDigi XML command assembly so string parameters are XML-escaped, integer commands use the integer helper, and generated RPC buffers are bounded before sending. ([8969235])
- Fixed FLDigi default modem handling so oversized configured modem names are rejected before fixed-buffer copies and generated KISS commands are bounded before sending. ([344c685])

### Verification

- `git diff --check` completed after the MULTIPSK debug format fix.
- `make -f makefile -B MULTIPSK.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` completed with existing unrelated warnings after the MULTIPSK debug format fix.
- `git diff --check` completed after the MULTIPSK RADIO/INUSE bounds fix.
- `make -f makefile -B MULTIPSK.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` completed with existing unrelated warnings after the MULTIPSK RADIO/INUSE bounds fix.
- `git diff --check` completed after the MULTIPSK default command bounds fix.
- `make -f makefile -B MULTIPSK.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` completed with existing unrelated warnings after the MULTIPSK default command bounds fix.
- `git diff --check` completed after the MULTIPSK64 bounds fix.
- `make -f makefile -B MULTIPSK64.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` is blocked by existing Linux integration errors in `MULTIPSK64.c`, including `PortConfig`/`TNCInfo` conflicts and missing `Interlock`/`BytesRXed`/`BytesTXed` members.
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
- `git diff --check` completed after the SCS Pactor debug format fix.
- `make -f makefile -B SCSPactor.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` completed with existing unrelated warnings after the SCS Pactor debug format fix.
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
- `make -f makefile -B FLDigi.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` completed with remaining existing unrelated warnings; the FLDigi UI frame reply overflow warning is cleared.
- `make -f makefile clean` completed before the final FLDigi UI frame reply verification build.
- `make -f makefile nomqtt` completed and linked `linbpq` after the FLDigi UI frame reply bounds fix.
- `make -f makefile -B FLDigi.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` completed with remaining existing unrelated warnings; the FLDigi XML command integer format warning is cleared.
- `make -f makefile clean` completed before the final FLDigi XML command verification build.
- `make -f makefile nomqtt` completed and linked `linbpq` after the FLDigi XML command bounds fix.
- `make -f makefile -B FLDigi.o CFLAGS='-DLINBPQ -MMD -g -fcommon -fasynchronous-unwind-tables -Wall -Wextra -Wformat -Wformat-security -Wstringop-overflow -Warray-bounds -DNOMQTT'` completed with remaining existing unrelated warnings; the FLDigi default modem command overflow warning is cleared.
- `make -f makefile clean` completed before the final FLDigi default modem verification build.
- `make -f makefile nomqtt` completed and linked `linbpq` after the FLDigi default modem bounds fix.

## 2026-05-22

Sources: commits listed on each fixed security line.

### Security

- Fixed WebMail attachment downloads so invalid requested attachment numbers are rejected before indexing fixed attachment arrays. ([54ee17a])
- Fixed WebMail message uploads so excess multipart attachments are rejected before indexing fixed attachment arrays. ([c051946])
- Fixed WebMail template form submissions so excess multipart fields are rejected before indexing fixed key/value arrays. ([874f364])
- Fixed WebMail delete requests so missing message IDs return a not-found page instead of dereferencing a missing message record. ([a5cab13])
- Fixed WebMail reply and quote-original requests so message ownership is checked before message content is exposed. ([f788b88])

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

- Fixed protected node API routes so read-only endpoints remain public while mutating and protected routes require a valid API key. ([bfff949])
- Fixed local include casing so quoted includes match the actual `rigresource.h` filename on case-sensitive Linux filesystems. ([bfff949])
- Fixed RHP session handle ownership so send, close, and status requests can only use handles owned by the current websocket session. ([75e083c])
- Fixed TriMode telnet debug logging so network/session text is logged with a literal format string. ([1946167])
- Fixed APRS debug logging so APRS-IS, RF, NMEA, AIS, and weather text is logged with a literal format string. ([4b31822])
- Fixed Telnet outward-connect command parsing and signon assembly so oversized tokens are rejected before they can overflow fixed buffers. ([62b037b])
- Fixed Telnet, relay, and CMS login logging so oversized remote username, password, and signon text is bounded before writing to stack log buffers. ([92f6e91])
- Fixed public node API port endpoints so invalid port numbers are rejected before indexing fixed port tables. ([207953b])
- Fixed BBS attachment download parsing so malformed or oversized stored attachment filenames are rejected before fixed-buffer copies. ([8bb38e5])
- Fixed RHP websocket receive forwarding so packet data is JSON-escaped into sized buffers before websocket frames are built. ([77a294a])

### Linux Build and Runtime

- Updated the GNU/Linux build workflow so `make all` compiles and links `linbpq` without running `sudo`. ([66478d8])
- Added a `make setcap` target that prints the required root-only `setcap` command instead of running it. ([66478d8])
- Added build artifact ignores for object, dependency, binary, and map outputs. ([66478d8])
- Added `LOCATOR=NONE` to the local telnet-only example config so runtime smoke tests do not emit the missing locator warning. ([66478d8])
- Verified `./linbpq -h`, `ldd ./linbpq`, and a temporary loopback daemon runtime smoke test. ([66478d8], [fbf82a0])

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

- Fixed HTTP session cookie handling so `BPQSessionCookie=N...` is trusted only when it maps to a live session. ([3d14e3d])
- Fixed direct APRS message POST handling so remote requests require either local access or a valid session. ([3d14e3d])
- Fixed APRS callsign handling so missing callsign input is rejected instead of being dereferenced. ([3d14e3d])
- Fixed RHP websocket stream opens so they require an authenticated secure websocket session. ([3d14e3d])
- Fixed rig and RHP websocket control paths so cookie presence alone is not treated as authentication. ([3d14e3d])
- Replaced time-derived HTTP session keys with OS random bytes, with a fallback only if the OS random source fails. ([3d14e3d])
- Replaced `rand()` API token generation in `nodeapi.c` and `mailapi.c` with OS random bytes. ([3d14e3d])
- Added bounded copies for beacon destination, file, and text fields, and reject invalid beacon ports. ([3d14e3d])

### Web UI

- Added `/m6vpn.css` and `/m6vpn-ui.js` references across node, APRS, mail, webmail, chat, rig, status, and driver pages.
- Removed many hardcoded `background` and `bgcolor` attributes so common styling can be handled by the shared UI assets.

### Documentation

- Added `README.md` with security fix notes, patch generation command, and verification notes.

### Verification

- `make -B RHP.o` completed, with an existing warning in `RHP.c`.
- Full object checks for `HTTPcode.c`, `APRSCode.c`, and `nodeapi.c` are blocked by the existing `Rigresource.h` include case mismatch.
- `mailapi.c` object compile is blocked by the existing missing `dbghelp.h` include.

[8c62e11]: https://github.com/M6VPN/linbpq/commit/8c62e119e3df448958bf7c9b2e8b21ec37e89699
[a6872dd]: https://github.com/M6VPN/linbpq/commit/a6872ddfb58ced805b0a3519895b636f3b28c453
[4bc6c23]: https://github.com/M6VPN/linbpq/commit/4bc6c231dc914f048e933f89af4613ff0a87ce78
[5d0cc10]: https://github.com/M6VPN/linbpq/commit/5d0cc104a05829979a9807e2ab65d077dfc63621
[73a8e67]: https://github.com/M6VPN/linbpq/commit/73a8e67edf2fba824df514d74c5fb4b45f42a5f4
[8969235]: https://github.com/M6VPN/linbpq/commit/896923589e5d572d6940e51b5464c4a3e5189bad
[344c685]: https://github.com/M6VPN/linbpq/commit/344c685d5b6f74cfaca26f366d387aec6d5a279d
[b391328]: https://github.com/M6VPN/linbpq/commit/b39132848c8e009a9660e1510ca844c3e9b8a026
[a30cbd3]: https://github.com/M6VPN/linbpq/commit/a30cbd3101867692436824a9b7ff02de9234ea96
[57edf61]: https://github.com/M6VPN/linbpq/commit/57edf61cc5eb2498d6f127a152d78f676dc30535
[bc3d541]: https://github.com/M6VPN/linbpq/commit/bc3d5410cf0c3398bcbb5ed61ddb9610d7d08daf
[e96206a]: https://github.com/M6VPN/linbpq/commit/e96206aec4c6c419acde028aa06d8b173011bf54
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
