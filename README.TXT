ANetRLogin v0.2.0

ANetRLogin is an outbound RLogin door for classic DOS BBS software.

It was created for A-Net Online’s Spitfire BBS, allowing callers on a
DOS BBS to connect to modern RLogin-enabled BBSes and game servers while
continuing to use the BBS’s normal FOSSIL/door interface.

ANetRLogin uses two small programs:

-   ANETRLGN.EXE - 16-bit DOS door. Runs from the BBS and communicates
    with the caller through OpenDoors/FOSSIL.
-   ANETRLB.EXE - 32-bit Windows bridge. Makes the outbound TCP/RLogin
    connection and relays data between the DOS door and remote server.

The bridge uses a destination allowlist. Callers select only a
configured destination ID; they cannot supply arbitrary hostnames or
ports.

Requirements

-   DOS-compatible BBS that creates a supported door drop file
-   FOSSIL driver / virtual modem environment used by the BBS
-   Windows host capable of running the 32-bit ANETRLB.EXE bridge
-   Network access from the Windows host to the configured RLogin
    server(s)

ANetRLogin was developed and tested with Spitfire using DOOR.SYS and a
Windows 7 32-bit host.

Included Files

ANETRLGN.EXE DOS door ANETRLB.EXE Windows RLogin bridge ANETRLGN.CFG
OpenDoors/BBS configuration ANETRLGN.INI RLogin destination allowlist
README.TXT Overview and usage INSTALL.TXT Installation instructions
SETUP.TXT Configuration and multi-destination examples FILE_ID.DIZ BBS
file description

How It Works

Caller -> BBS -> ANETRLGN.EXE -> local per-node IPC -> ANETRLB.EXE ->
TCP/RLogin -> configured remote BBS or game server

ANETRLGN.EXE is launched with a destination ID:

    ANETRLGN.EXE ANETGAMES

ANETRLB.EXE looks up ANETGAMES in ANETRLGN.INI and makes the connection.

Multiple destinations may be configured. The same ANETRLGN.EXE is used
for every destination; only the command-line destination ID changes.

Disconnecting

Press ESC three times quickly while inside ANetRLogin to disconnect the
outbound RLogin session and return to the BBS.

Security

RLogin is a plaintext protocol. ANetRLogin does not add encryption to
it. Use RLogin only to systems you trust and preferably across
trusted/private networks or VPN paths when appropriate.

ANetRLogin deliberately does NOT accept an arbitrary hostname or port
from the caller. All remote systems must be explicitly configured in
ANETRLGN.INI.

Version 0.2.0

v0.2.0 is the low-latency release. It improves interactive
responsiveness with non-blocking network relay behavior, TCP_NODELAY,
faster IPC polling, and immediate handling of terminal traffic.

ANetRLogin By: A-Net Online Sysop: StingRay
https://a-net.online
https://bbs.a-net.fyi

telnet://bbs.a-net.online:1337
ssh://bbs.a-net.online:1338
