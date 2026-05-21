# LinBPQ Hardening Fork

This repository is a working fork/copy of LinBPQ used for security hardening, Linux build verification, and small maintainability fixes. The tree tracks confirmed security fixes, known open problems, and command-level verification notes as the audit progresses.

The upstream LinBPQ project by John Wiseman (G8BPQ) remains the original source and base for this work. Review diffs, configuration, and local runtime requirements before deploying changes from this fork.

Modernisation work is also a focus of this fork.

73 de M6VPN

## Contents

- [Project status](#project-status)
- [What this fork changes](#what-this-fork-changes)
- [Security tracking](#security-tracking)
- [Build and runtime verification](#build-and-runtime-verification)
- [Applying changes to another LinBPQ tree](#applying-changes-to-another-linbpq-tree)
- [Notes for operators](#notes-for-operators)
- [Disclaimer](#disclaimer)

## Project status

This is an active hardening and audit tree. Fixes are applied incrementally, with each pass focused on confirmed issues and narrow verification.

The current audit state is tracked in [security.md](security.md). Dated changes and verification notes are tracked in [changelog.md](changelog.md).

## What this fork changes

This fork currently focuses on:

- HTTP, session, and API authentication fixes
- Websocket and RHP hardening
- Telnet and APRS logging and buffer fixes
- Node API bounds checks
- BBS and mail attachment filename checks
- Linux build and runtime smoke-test improvements
- A modern design for the web interface, originally for M6VPN-7/1

See [security.md](security.md) and [changelog.md](changelog.md) for the detailed fix history and open items.

## Security tracking

[security.md](security.md) is the source of truth for fixed security issues, known open problems, and verification status.

[changelog.md](changelog.md) records dated changes, recent commits, and command results from audit and verification passes.

## Build and runtime verification

Basic Linux build and help checks:

```bash
make all
./linbpq -h
```

Local smoke test using the telnet-only example config:

```bash
rm -rf /tmp/linbpq-runtime-test
mkdir -p /tmp/linbpq-runtime-test/config /tmp/linbpq-runtime-test/data /tmp/linbpq-runtime-test/log
cp bpq32.cfg.example /tmp/linbpq-runtime-test/config/bpq32.cfg
timeout 12s ./linbpq -c /tmp/linbpq-runtime-test/config -d /tmp/linbpq-runtime-test/data -l /tmp/linbpq-runtime-test/log
pgrep -af linbpq
```

This is a runtime smoke test, not full regression testing. `make all` relinks `linbpq` and may leave Linux file capabilities unset.

## Applying changes to another LinBPQ tree

Review [changelog.md](changelog.md) first to identify the relevant fixes and verification context. Then inspect or export the corresponding changes with normal Git tooling, such as:

```bash
git diff
git format-patch
```

Apply changes to another LinBPQ tree only after reviewing local configuration, platform differences, and any open problems listed in [security.md](security.md).

## Notes for operators

Review configuration before exposing HTTP, Telnet, API, WebSocket, Mail, or BBS services. Change example credentials before use.

If Linux capabilities are needed after a relink, reapply them manually:

```bash
sudo setcap "CAP_NET_ADMIN=ep CAP_NET_RAW=ep CAP_NET_BIND_SERVICE=ep" ./linbpq
```

This command is operator-run and is not required for a normal compile.

## Disclaimer

This fork is provided for hardening, testing, and review. It is not a claim that LinBPQ is fully audited or production-secure. Operators are responsible for testing, configuration review, and deployment risk in their own environment.
