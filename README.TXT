ANetRLogin v0.2.5
===================

Outbound RLogin for classic DOS BBS systems.

ANetRLogin consists of:

  ANETRLGN.EXE  16-bit DOS/OpenDoors/FOSSIL door
  ANETRLB.EXE   32-bit Windows RLogin bridge/multiplexer
  ANETRLV.EXE   32-bit Windows read-only ANSI/CP437 local viewer

The DOS door keeps the caller path compatible with classic BBS software. The
bridge owns TCP/RLogin. The optional local viewer mirrors remote ANSI/CP437 in
a separate Win32 window and never sits in the caller path.

FEATURES
--------
- Up to 10 BBS nodes through one bridge
- Up to 32 allowlisted destinations
- %USER%, %ALIAS%, %PASSWORD% macros
- Tested Synchronet mapping: PASSWORD first, USER second
- Non-blocking/TCP_NODELAY low-latency relay
- Per-node IPC/session isolation
- Optional per-node local viewer
- Configurable viewer geometry, including 80 columns and configurable widths
- ESC ESC ESC manual disconnect
- Password never displayed by bridge/viewer logs or UI

A-Net Game Server example:

  [general]
  local_viewer=1
  viewer_cols=80
  viewer_rows=25

  [destination ANETGAMES]
  name=A-Net Game Server
  host=game.a-net-online.lol
  port=513
  client_user=%PASSWORD%
  server_user=%USER%
  term=ansi/115200
  expect_ack=1
  viewer_cols=80
  viewer_rows=25

The viewer geometry affects only ANETRLV.EXE, not the caller terminal.

MULTI-NODE
----------
Use a node-specific BBSDir for the live DOOR.SYS and the SAME shared DoorDir
for all nodes. One ANETRLB.EXE services all active nodes.

A-Net examples:
  Node 1 BBSDir C:\SF\SFEXTEN2
  Node 2 BBSDir C:\SF2\SFEXTEN2
  Shared DoorDir C:\SF\ANETRLGN

SECURITY
--------
Destinations are allowlisted in ANETRLGN.INI. Callers cannot supply arbitrary
hosts/ports. RLogin is plaintext, so %PASSWORD% is transmitted unencrypted
when that macro is used. Prefer trusted/private/VPN paths where practical.

See INSTALL.TXT and SETUP.TXT for full details.

A-Net Online / StingRay
https://a-net.online
https://bbs.a-net.fyi
telnet://bbs.a-net.online:1337
ssh://bbs.a-net.online:1338
