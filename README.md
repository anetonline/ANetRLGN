<img width="1389" height="703" alt="image" src="https://github.com/user-attachments/assets/d34644b6-7e3a-4384-969e-ef3da9e7d8e8" />


# ANetRLogin

### Outbound RLogin for Classic DOS BBS Systems

**ANetRLogin v0.2.1 — User/Password + Multi-Node Release**

ANetRLogin is an outbound **RLogin door for classic DOS BBS software**.

It was created for **A-Net Online's Spitfire BBS**, allowing callers on a
DOS BBS to connect to modern RLogin-enabled BBSes and game servers while
continuing to use the BBS's normal **FOSSIL / door interface**.

ANetRLogin combines a **16-bit DOS door built with the OpenDoors Door
Programming Toolkit** with a **32-bit Windows bridge**, bringing TCP/IP
RLogin connectivity to classic BBS software without requiring the DOS BBS
itself to provide a TCP/IP stack.

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
- OpenDoors drop-file and caller integration
- DOOR.SYS support
- FOSSIL communications
- BBS username, alias/handle, and password mapping
- `%USER%`, `%ALIAS%`, and `%PASSWORD%` macros
- Multiple RLogin destinations from one door executable
- Up to **10 BBS nodes**
- Up to **32 configured destinations**
- One shared Win32 bridge for all active nodes
- Per-node IPC/session isolation
- Destination allowlist for outbound connections
- Configurable RLogin client/server identity fields
- Configurable terminal/speed field
- ANSI terminal relay
- Non-blocking network I/O
- `TCP_NODELAY`
- Low-latency DOS ↔ Windows IPC polling
- Quick **ESC ESC ESC** disconnect back to the BBS
- No arbitrary caller-supplied hostnames or ports
- Password values are never displayed by the bridge

---

## 🖥️ How It Works

```text
                    ANetRLogin Architecture

                       Caller
                         │
                         ▼
                ┌─────────────────┐
                │    DOS BBS      │
                │   (Spitfire)    │
                └────────┬────────┘
                         │
                  DOOR.SYS / FOSSIL
                         │
                         ▼
                ┌─────────────────┐
                │    OpenDoors    │
                │     Toolkit     │
                └────────┬────────┘
                         │
                         ▼
                ┌─────────────────┐
                │  ANETRLGN.EXE   │
                │   16-bit DOS    │
                └────────┬────────┘
                         │
                  Local Per-Node IPC
                         │
                         ▼
                ┌─────────────────┐
                │   ANETRLB.EXE   │
                │  32-bit Win32   │
                │     Bridge      │
                └────────┬────────┘
                         │
                    TCP / RLogin
                         │
                         ▼
                ┌─────────────────┐
                │   Remote BBS    │
                │       or        │
                │   Game Server   │
                └─────────────────┘
```

`ANETRLGN.EXE` runs as the DOS BBS door and communicates with the caller
through OpenDoors and the BBS's FOSSIL environment.

`ANETRLB.EXE` runs under Windows and handles DNS, TCP, and the RLogin
handshake.

The two programs communicate through local **per-node IPC files**, allowing
the DOS side to remain compatible with the classic BBS environment while
the Win32 side handles modern networking.

---

## 🚪 OpenDoors

ANetRLogin's 16-bit DOS door is built using the **OpenDoors Door Programming
Toolkit**.

OpenDoors provides the classic BBS interface used by `ANETRLGN.EXE`,
including:

- Door drop-file handling
- Caller information
- User name and alias/handle information
- User password information when supplied by the drop file
- BBS node information
- Carrier detection
- Keyboard input
- Terminal output
- FOSSIL communications

For A-Net Online's Spitfire installation, OpenDoors reads Spitfire's
`DOOR.SYS` and communicates with the caller through the BBS's
FOSSIL/virtual-modem environment.

ANetRLogin then makes the values available to the destination configuration:

```text
%USER%       BBS user name
%ALIAS%      BBS alias / handle
%PASSWORD%   BBS password
```

### OpenDoors Project

ANetRLogin uses the open-source **OpenDoors Door Programming Toolkit**:

https://github.com/RealDeuce/OpenDoors

OpenDoors does **not** need to be separately installed by users of the
compiled ANetRLogin release. The required OpenDoors functionality is
incorporated into the compiled DOS door.

Our thanks to the OpenDoors developers and contributors for preserving and
maintaining this extremely useful toolkit for classic BBS door development.

---

## 📋 Requirements

- DOS-compatible BBS that creates a supported door drop file
- FOSSIL driver / virtual modem environment used by the BBS
- Windows host capable of running the 32-bit `ANETRLB.EXE` bridge
- Network access from the Windows host to the configured RLogin server(s)

ANetRLogin was developed and tested with:

- **Spitfire BBS**
- **DOOR.SYS**
- **Windows 7 32-bit**
- FOSSIL / virtual modem environment
- SyncTERM
- TCP/RLogin

---

## 📦 Included Files

| File | Description |
|---|---|
| `ANETRLGN.EXE` | 16-bit DOS RLogin door |
| `ANETRLB.EXE` | 32-bit Windows RLogin bridge |
| `ANETRLGN.CFG` | OpenDoors / BBS configuration |
| `ANETRLGN.INI` | RLogin destination allowlist |
| `README.TXT` | Program overview and usage |
| `INSTALL.TXT` | Installation instructions |
| `SETUP.TXT` | Configuration and multi-node examples |
| `FILE_ID.DIZ` | Classic BBS file description |

---

# 🚀 Installation

The following example uses the directory layout from the live
**A-Net Online Spitfire BBS** installation.

### Shared ANetRLogin directory

```text
C:\SF\ANETRLGN
```

Place the shared bridge and destination configuration here:

```text
ANETRLB.EXE
ANETRLGN.INI
```

For a single-node setup, `ANETRLGN.EXE` and `ANETRLGN.CFG` may also live
directly in this directory.

---

## ⚙️ OpenDoors / Spitfire Configuration

`ANETRLGN.CFG` tells OpenDoors where that node's live `DOOR.SYS` is located
and where ANetRLogin's shared IPC directory is located.

For the A-Net Online Spitfire **extended door menu**, Node 1 uses:

```text
BBSDir C:\SF\SFEXTEN2
DoorDir C:\SF\ANETRLGN

DisableLogging
```

The important rule for multi-node systems is:

```text
BBSDir  = node-specific drop-file directory
DoorDir = shared ANetRLogin IPC directory
```

---

# 🌐 Configuring an RLogin Destination

All permitted destinations are configured in:

```text
ANETRLGN.INI
```

## Tested Synchronet-Compatible User/Password Mapping

A-Net Online's Game Server is Synchronet-based. The tested working mapping is:

```ini
[destination ANETGAMES]
name=A-Net Game Server
host=game.a-net-online.lol
port=513

client_user=%PASSWORD%
server_user=%USER%

term=ansi/115200
expect_ack=1
```

**The order is important.**

For a Synchronet-compatible RLogin server, the password is placed in the
RLogin `client_user` field and the BBS username is placed in the
`server_user` field.

This is the ANetRLogin equivalent of Synchronet's normal `rlogin.js -p`
style of passing the caller's identity and password to another Synchronet
RLogin server.

The destination ID in this example is:

```text
ANETGAMES
```

The BBS launches it with:

```text
ANETRLGN.EXE ANETGAMES
```

`ANETRLB.EXE` looks up `ANETGAMES` in `ANETRLGN.INI` and makes the configured
outbound RLogin connection.

> Different RLogin servers may expect the two RFC1282 identity fields in a
> different order. ANetRLogin therefore keeps both fields fully configurable.

---

## 👤 Caller Identity Macros

ANetRLogin v0.2.1 provides three macros:

```text
%USER%       BBS user name
%ALIAS%      BBS alias / handle
%PASSWORD%   BBS password
```

These may be used in either RLogin identity field.

Examples:

```ini
client_user=%PASSWORD%
server_user=%USER%
```

or:

```ini
client_user=%PASSWORD%
server_user=%ALIAS%
```

or for a server that does not use a password:

```ini
client_user=%USER%
server_user=%ALIAS%
```

The mapping is destination-specific, so one ANetRLogin installation can
connect to servers with different RLogin conventions.

### Synchronet `-p` and `-h`

Synchronet's outbound `rlogin.js` supports plain-password (`-p`) and
hashed-password (`-h`) modes.

ANetRLogin v0.2.1 currently implements the **plain-password** case by using
`%PASSWORD%`.

ANetRLogin does **not currently implement Synchronet's `-h` salted/hashed
password algorithm**. Do not substitute an ordinary hash and assume it is
compatible.

---

# 🕹️ Multiple RLogin Destinations

A single ANetRLogin installation can provide many outbound destinations.

```ini
[destination ANETGAMES]
name=A-Net Game Server
host=game.a-net-online.lol
port=513
client_user=%PASSWORD%
server_user=%USER%
term=ansi/115200
expect_ack=1

[destination OTHERBBS]
name=Another RLogin BBS
host=bbs.example.com
port=513
client_user=%USER%
server_user=%ALIAS%
term=ansi/115200
expect_ack=1
```

Corresponding BBS commands:

```text
ANETRLGN.EXE ANETGAMES
ANETRLGN.EXE OTHERBBS
```

The same executable is used for every destination.

ANetRLogin supports up to **32 configured destinations**.

> Restart `ANETRLB.EXE` after changing `ANETRLGN.INI`.

---

# 👥 Multi-Node Operation

ANetRLogin supports up to **10 BBS nodes** through a single
`ANETRLB.EXE` bridge.

Multi-node operation has been live-tested on A-Net Online with two
simultaneous Spitfire nodes connected to the A-Net Game Server.

Each node uses its own `ANETRLGN.EXE` / `ANETRLGN.CFG` launch directory when
its `DOOR.SYS` resides in a different location, while all nodes share the
same `DoorDir`.

For the live A-Net Online Spitfire extended door menu:

```text
Node 1 DOOR.SYS:
C:\SF\SFEXTEN2\DOOR.SYS

Node 2 DOOR.SYS:
C:\SF2\SFEXTEN2\DOOR.SYS
```

A convenient layout is:

```text
C:\SF\ANETRLGN\
    ANETRLB.EXE
    ANETRLGN.INI

C:\SF\ANETRLGN\NODE1\
    ANETRLGN.EXE
    ANETRLGN.CFG

C:\SF\ANETRLGN\NODE2\
    ANETRLGN.EXE
    ANETRLGN.CFG
```

Node 1 `ANETRLGN.CFG`:

```text
BBSDir C:\SF\SFEXTEN2
DoorDir C:\SF\ANETRLGN

DisableLogging
```

Node 2 `ANETRLGN.CFG`:

```text
BBSDir C:\SF2\SFEXTEN2
DoorDir C:\SF\ANETRLGN

DisableLogging
```

Door commands:

```text
Node 1:
C:\SF\ANETRLGN\NODE1\ANETRLGN.EXE ANETGAMES

Node 2:
C:\SF\ANETRLGN\NODE2\ANETRLGN.EXE ANETGAMES
```

Only **one `ANETRLB.EXE` bridge** is required.

Each node receives its own IPC files:

```text
Node 1: ARL01.*
Node 2: ARL02.*
...
Node 10: ARL10.*
```

This keeps simultaneous RLogin sessions isolated while sharing one bridge.

---

# ▶️ Starting the Windows Bridge

From a Windows command prompt:

```bat
CD /D C:\SF\ANETRLGN
ANETRLB.EXE
```

Leave the bridge running while ANetRLogin is available to callers.

Restart the bridge after changing `ANETRLGN.INI`.

---

# 🔙 Disconnecting

While connected through ANetRLogin, press:

```text
ESC ESC ESC
```

three times quickly.

ANetRLogin disconnects the outbound RLogin session and returns the caller
to the BBS.

---

# ⚡ v0.2.x Low-Latency Relay

The low-latency relay engine uses:

- Non-blocking network I/O
- `TCP_NODELAY`
- Fast DOS/Win32 IPC polling
- Immediate outbound keystroke processing
- Immediate draining of available inbound network traffic
- Reduced idle polling delays

The result is responsive interactive ANSI/RLogin use even through the
16-bit DOS door → Win32 bridge architecture.

---

# 🔐 Security

ANetRLogin uses a strict **destination allowlist**.

A caller cannot provide an arbitrary hostname or TCP port. The sysop controls
the actual destination in `ANETRLGN.INI`.

### Password handling

When `%PASSWORD%` is used:

- The bridge does **not display the password**
- The bridge does **not include the password in normal log messages**
- The password is used only to construct the configured RLogin handshake
- The local DOS-to-Windows request is transient and is consumed by the bridge

### RLogin Security Notice

RLogin is a **plaintext protocol**.

When `%PASSWORD%` is selected, the caller's password is sent across the
network as plaintext in the RLogin handshake.

Use RLogin only with systems you trust and preferably across trusted/private
networks or VPN paths where appropriate.

Synchronet's `-h` mode avoids transmitting the original password by sending
a salted hash instead. ANetRLogin v0.2.1 does not yet implement that mode.

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
Synchronet RLogin server
A-Net Game Server
```

Multi-node testing has also been completed with **two simultaneous Spitfire
nodes**, both connected through one `ANETRLB.EXE` bridge.

---

# 🙏 Credits

**ANetRLogin** was created by:

**Jerry Reed (StingRay)**  
**A-Net Online**

The 16-bit DOS portion uses the **OpenDoors Door Programming Toolkit**:

https://github.com/RealDeuce/OpenDoors

Special thanks to the OpenDoors developers and contributors for preserving
and maintaining a modern toolkit for classic BBS door development.

---

# 📜 License

**MIT License**

Copyright © 2026 **A-Net Online / Jerry Reed (StingRay)**

See `LICENSE` for details.

---

# 🌐 A-Net Online

**ANetRLogin**  
By **A-Net Online**  
Sysop: **StingRay**

### Web

- https://a-net.online
- https://bbs.a-net.fyi

### A-Net Online BBS

```text
Telnet : bbs.a-net.online:1337
SSH    : bbs.a-net.online:1338
```

**Bringing modern RLogin connectivity to classic DOS BBS software.**
