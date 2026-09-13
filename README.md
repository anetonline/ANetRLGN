<img width="1389" height="703" alt="image" src="https://github.com/user-attachments/assets/d34644b6-7e3a-4384-969e-ef3da9e7d8e8" />

<img width="734" height="344" alt="image" src="https://github.com/user-attachments/assets/23d75448-978f-4a93-9581-f64cd126cbea" />


# ANetRLogin

### Outbound RLogin for Classic DOS BBS Systems

**ANetRLogin v0.2.5 — Multi-Node, User/Password, Low-Latency + Local Viewer**

ANetRLogin is an outbound **RLogin door for classic DOS BBS software**.

It was created for **A-Net Online's Spitfire BBS**, allowing callers on a
DOS BBS to connect to modern RLogin-enabled BBSes and game servers while
continuing to use the BBS's normal **FOSSIL / door interface**.

ANetRLogin combines a **16-bit DOS door built with the OpenDoors Door
Programming Toolkit**, a **32-bit Windows RLogin bridge**, and an optional
**32-bit Windows ANSI/CP437 local viewer**.

---

## 🎬 Demo

Watch **ANetRLogin in action from a live Spitfire BBS session**.

The caller is connected to A-Net Online's Spitfire BBS using SyncTERM,
enters the Doors menu, launches ANetRLogin, and connects to a remote
RLogin service.

https://github.com/user-attachments/assets/3e877981-5124-40b9-8b2a-64e69a34fb53

---

## ✨ Features

- **16-bit DOS door built with OpenDoors**
- **32-bit Windows RLogin bridge**
- **32-bit Windows local ANSI/CP437 viewer**
- OpenDoors drop-file and caller integration
- DOOR.SYS support
- FOSSIL communications
- `%USER%`, `%ALIAS%`, and `%PASSWORD%` caller macros
- Synchronet-compatible tested **password-first / user-second** mapping
- Multiple RLogin destinations from one door executable
- Up to **10 BBS nodes**
- Up to **32 configured destinations**
- One shared Win32 bridge for all active nodes
- One optional local viewer window per active node
- Configurable viewer geometry, including **80-column and configurable-width** sessions
- Per-node IPC/session isolation
- Strict destination allowlist
- Configurable RLogin client/server identity fields
- Configurable terminal/speed field
- ANSI/CP437 terminal relay
- Non-blocking network I/O and `TCP_NODELAY`
- Low-latency DOS ↔ Windows IPC polling
- Quick **ESC ESC ESC** manual disconnect back to the BBS
- Password values are never displayed by the bridge or local viewer

---

## 🖥️ How It Works

```text
                       Caller
                         │
                         ▼
                ┌─────────────────┐
                │    DOS BBS      │
                │   (Spitfire)    │
                └────────┬────────┘
                         │ DOOR.SYS / FOSSIL
                         ▼
                ┌─────────────────┐
                │    OpenDoors    │
                └────────┬────────┘
                         ▼
                ┌─────────────────┐
                │  ANETRLGN.EXE   │
                │   16-bit DOS    │
                └────────┬────────┘
                         │ per-node IPC
                         ▼
                ┌─────────────────┐
                │   ANETRLB.EXE   │──────────────┐
                │  32-bit Win32   │              │ mirror only
                └────────┬────────┘              ▼
                         │ TCP / RLogin     ┌──────────────┐
                         ▼                  │ ANETRLV.EXE  │
                ┌─────────────────┐         │ local viewer │
                │ Remote BBS/Game │         └──────────────┘
                └─────────────────┘
```

`ANETRLGN.EXE` remains responsible only for the DOS/OpenDoors/FOSSIL caller
path. `ANETRLB.EXE` owns networking and the RLogin handshake. `ANETRLV.EXE`
is a separate read-only monitor and is **not** in the caller data path.

Closing a local viewer does not disconnect the caller.

---

## 🚪 OpenDoors

ANetRLogin's 16-bit DOS door is built using the **OpenDoors Door Programming
Toolkit**. OpenDoors provides drop-file handling, caller information, user
name/alias/password fields when available, node information, carrier detection,
keyboard input, terminal output, and FOSSIL communications.

ANetRLogin makes these caller values available as destination macros:

```text
%USER%       BBS user name
%ALIAS%      BBS alias / handle
%PASSWORD%   BBS password
```

OpenDoors project:
https://github.com/RealDeuce/OpenDoors

OpenDoors does **not** need to be installed separately to use the compiled
ANetRLogin DOS door.

---

## 📋 Requirements

- DOS-compatible BBS that creates a supported door drop file
- FOSSIL driver / virtual-modem environment used by the BBS
- Windows host capable of running 32-bit Win32 programs
- Network access from the Windows host to configured RLogin server(s)

Developed and live-tested with **Spitfire BBS**, **DOOR.SYS**, **Windows 7
32-bit**, OpenDoors/FOSSIL, SyncTERM, Synchronet RLogin, and A-Net Game Server.

---

## 📦 Included Files

| File | Description |
|---|---|
| `ANETRLGN.EXE` | 16-bit DOS RLogin door |
| `ANETRLB.EXE` | 32-bit Windows RLogin bridge/multiplexer |
| `ANETRLV.EXE` | 32-bit Windows read-only ANSI/CP437 local viewer |
| `ANETRLGN.CFG` | OpenDoors / BBS configuration |
| `ANETRLGN.INI` | General settings + RLogin destination allowlist |
| `README.TXT` | Plain-text overview |
| `INSTALL.TXT` | Installation instructions |
| `SETUP.TXT` | Configuration and multi-node examples |
| `FILE_ID.DIZ` | Classic BBS file description |

---

# 🚀 Installation

A-Net Online uses a shared ANetRLogin directory:

```text
C:\SF\ANETRLGN
```

Shared files:

```text
ANETRLB.EXE
ANETRLV.EXE
ANETRLGN.INI
```

Each Spitfire node can have its own DOS door/config directory so OpenDoors can
read the correct node-specific `DOOR.SYS`.

For the live A-Net extended-door setup:

```text
Node 1 DOOR.SYS: C:\SF\SFEXTEN2\DOOR.SYS
Node 2 DOOR.SYS: C:\SF2\SFEXTEN2\DOOR.SYS
```

Node 1 `ANETRLGN.CFG`:

```text
BBSDir C:\SF\SFEXTEN2
DoorDir C:\SF\ANETRLGN
DisableLogging
```

Node 2:

```text
BBSDir C:\SF2\SFEXTEN2
DoorDir C:\SF\ANETRLGN
DisableLogging
```

Rule:

```text
BBSDir  = node-specific directory containing the live DOOR.SYS
DoorDir = shared ANetRLogin bridge/IPC directory
```

---

# 🌐 ANETRLGN.INI

## General / Local Viewer

```ini
[general]
local_viewer=1
viewer_cols=80
viewer_rows=25
```

`local_viewer=1` automatically opens one `ANETRLV.EXE` window for each active
node. Set it to `0` to disable local monitor windows.

`viewer_cols` and `viewer_rows` are defaults. A destination may override them.
Supported local-viewer range is **40-160 columns** and **20-60 rows**.

## Tested A-Net Game Server / Synchronet Mapping

```ini
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
```

**The tested Synchronet order is PASSWORD FIRST, USER SECOND.**

For this target, `%PASSWORD%` is placed in RFC1282's `client_user` field and
`%USER%` in `server_user`. Other RLogin servers may use those fields
differently, so ANetRLogin leaves both configurable.

The local-viewer geometry above does **not** alter what the caller receives;
it only controls the separate local monitor window.

### Synchronet `-p` and `-h`

ANetRLogin's `%PASSWORD%` support provides the plain-password behavior needed
for Synchronet-style `-p` login. Synchronet also supports a salted/hashed `-h`
mode; ANetRLogin does **not currently implement that hashing format**.

---

# 🕹️ Multiple Destinations

```ini
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

[destination OTHERBBS]
name=Another RLogin BBS
host=bbs.example.com
port=513
client_user=%USER%
server_user=%ALIAS%
term=ansi/115200
expect_ack=1
viewer_cols=80
viewer_rows=25
```

BBS menu entries:

```text
ANETRLGN.EXE ANETGAMES
ANETRLGN.EXE OTHERBBS
```

Callers only supply the destination ID. They cannot choose arbitrary hosts or
ports. ANetRLogin supports up to **32 configured destinations**.

Restart `ANETRLB.EXE` after editing `ANETRLGN.INI`.

---

# 👥 Multi-Node Operation

ANetRLogin supports up to **10 BBS nodes** through one `ANETRLB.EXE` bridge.
Multi-node operation has been live-tested on A-Net Online with two simultaneous
Spitfire nodes connected to A-Net Game Server.

Recommended layout:

```text
C:\SF\ANETRLGN\
    ANETRLB.EXE
    ANETRLV.EXE
    ANETRLGN.INI

C:\SF\ANETRLGN\NODE1\
    ANETRLGN.EXE
    ANETRLGN.CFG

C:\SF\ANETRLGN\NODE2\
    ANETRLGN.EXE
    ANETRLGN.CFG
```

Door commands:

```text
Node 1: C:\SF\ANETRLGN\NODE1\ANETRLGN.EXE ANETGAMES
Node 2: C:\SF\ANETRLGN\NODE2\ANETRLGN.EXE ANETGAMES
```

Only **one bridge** is required. Per-node files keep sessions isolated:

```text
Node 1: ARL01.*
Node 2: ARL02.*
...
Node 10: ARL10.*
```

When the local viewer is enabled, the bridge also creates read-only monitor
spools such as `ARL01.VWR` / `ARL01.VMI` and launches one viewer per active
node.

---

# 🖼️ Local ANSI/CP437 Viewer

The separate viewer was added because raw local echo inside the DOS door did
not provide a reliable ANSI display and could interfere with normal door
behavior. The caller relay is therefore left alone.

```text
Caller path:  ANETRLB -> ANETRLGN -> FOSSIL -> caller
Viewer path:  ANETRLB -> ARLnn.VWR -> ANETRLV
```

`ANETRLV.EXE` is read-only. It cannot send keys to the remote system, and
closing it does not disconnect the BBS user.

### v0.2.5: 80x25 exact-size fix

v0.2.4 proved the viewer architecture live, but the viewer used a hard-coded
80-column terminal and cut off the right side of 80-column Synchronet
screens. v0.2.5 makes the viewer geometry configurable and sizes the Win32
client area from the actual Terminal font metrics with `AdjustWindowRectEx`.

For A-Net Game Server use:

```ini
viewer_cols=80
viewer_rows=25
```

For ordinary 80-column systems use `80`/`25` or omit the destination override.

---

# ▶️ Starting the Bridge

```bat
CD /D C:\SF\ANETRLGN
ANETRLB.EXE
```

Leave it running while callers use ANetRLogin. With `local_viewer=1`, the
bridge automatically launches `ANETRLV.EXE` when a node connects.

---

# 🔙 Disconnecting

A remote service may exit normally and return control to ANetRLogin. For a
manual/emergency disconnect, press:

```text
ESC ESC ESC
```

three times quickly to disconnect the outbound RLogin session and return to
the BBS.

---

# ⚡ Low-Latency Relay

The v0.2.x relay uses non-blocking network I/O, `TCP_NODELAY`, fast IPC
polling, immediate outbound keystroke processing, and immediate draining of
available inbound traffic. The local viewer is a separate mirror and does not
sit in this latency-sensitive caller path.

---

# 🔐 Security

ANetRLogin uses a strict destination allowlist. A caller cannot provide an
arbitrary hostname or TCP port.

When `%PASSWORD%` is used:

- the bridge does **not display the password**;
- normal bridge logs do **not contain the password**;
- viewer metadata does **not contain the password**;
- `ANETRLV.EXE` never displays the password;
- the transient DOS-to-Windows request is consumed by the bridge.

RLogin itself is a **plaintext protocol**. `%PASSWORD%` therefore travels over
the RLogin connection in plaintext. Use systems you trust and private/VPN
paths where practical.

---

# 🏁 Live Tested

ANetRLogin has been tested live with:

```text
Spitfire BBS
Windows 7 32-bit
OpenDoors
DOOR.SYS
FOSSIL / virtual modem environment
SyncTERM
Synchronet RLogin
A-Net Game Server
Two simultaneous Spitfire nodes
Separate Win32 local viewer
```

---

# 🙏 Credits

**ANetRLogin** was created by **Jerry Reed (StingRay) / A-Net Online**.

The 16-bit DOS portion uses the **OpenDoors Door Programming Toolkit**:
https://github.com/RealDeuce/OpenDoors

Special thanks to the OpenDoors developers and contributors for preserving and
maintaining a modern toolkit for classic BBS door development.

---

# 📜 License

**MIT License**  
Copyright © 2026 **A-Net Online / Jerry Reed (StingRay)**

See `LICENSE` for details.

---

# 🌐 A-Net Online

- https://a-net.online
- https://bbs.a-net.fyi

```text
Telnet : bbs.a-net.online:1337
SSH    : bbs.a-net.online:1338
```

**Bringing modern RLogin connectivity to classic DOS BBS software.**
