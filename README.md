# ANetRLogin

### Outbound RLogin for Classic DOS BBS Systems

**ANetRLogin v0.2.0 — Low-Latency Release**

ANetRLogin is an outbound **RLogin door for classic DOS BBS software**.

It was created for **A-Net Online's Spitfire BBS**, allowing callers on a
DOS BBS to connect to modern RLogin-enabled BBSes and game servers while
continuing to use the BBS's normal **FOSSIL / door interface**.

ANetRLogin combines a **16-bit DOS door** with a **32-bit Windows bridge**,
bringing TCP/IP RLogin connectivity to classic BBS software without requiring
the DOS BBS itself to provide a TCP/IP stack.

---

## 🎬 Demo

Watch **ANetRLogin in action from a live Spitfire BBS session**.

The caller is connected to A-Net Online's Spitfire BBS using SyncTERM,
enters the Doors menu, launches ANetRLogin, and connects to a remote
RLogin service.

https://github.com/user-attachments/assets/3e877981-5124-40b9-8b2a-64e69a34fb53

---

## ✨ Features

- **16-bit DOS door** for classic BBS environments
- **32-bit Windows RLogin bridge**
- OpenDoors / FOSSIL communication
- DOOR.SYS support
- Multiple RLogin destinations from a single door executable
- Up to **32 configured destinations**
- Up to **32 BBS nodes**
- Per-node sessions
- Destination allowlist for outbound connections
- `%USER%` and `%ALIAS%` RLogin username mapping
- Configurable hostname and TCP port per destination
- Configurable RLogin client/server username fields
- Configurable terminal/speed field
- ANSI terminal relay
- Non-blocking network I/O
- `TCP_NODELAY`
- Low-latency DOS ↔ Windows IPC polling
- Quick **ESC ESC ESC** disconnect back to the BBS
- No arbitrary caller-supplied hostnames or ports

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
through OpenDoors/FOSSIL.

`ANETRLB.EXE` runs under Windows and handles the actual TCP/RLogin
connection.

The two programs communicate through local **per-node IPC files**, allowing
the DOS side to remain compatible with the classic BBS environment while
the Win32 side handles modern networking.

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
| `SETUP.TXT` | Configuration and multi-destination examples |
| `FILE_ID.DIZ` | Classic BBS file description |

---

# 🚀 Installation

The following example uses the directory layout from the live
**A-Net Online Spitfire BBS** installation.

### Spitfire working directory

```text
C:\SF\WORK
```

This is where Spitfire creates the live `DOOR.SYS`.

### ANetRLogin directory

```text
C:\SF\ANETRLGN
```

Create the directory and place the ANetRLogin files inside it:

```text
ANETRLGN.EXE
ANETRLB.EXE
ANETRLGN.CFG
ANETRLGN.INI
```

---

## ⚙️ OpenDoors / Spitfire Configuration

For the tested A-Net Online Spitfire installation, `ANETRLGN.CFG` is:

```text
BBSDir C:\SF\WORK
DoorDir C:\SF\ANETRLGN

DisableLogging
```

### `BBSDir`

The directory where the BBS creates the caller's live `DOOR.SYS`.

### `DoorDir`

The ANetRLogin working directory shared by the DOS door and Windows bridge.

If your BBS uses different directories, adjust these paths accordingly.

---

# 🌐 Configuring an RLogin Destination

All permitted destinations are configured in:

```text
ANETRLGN.INI
```

For example, the **A-Net Game Server**:

```ini
[destination ANETGAMES]
name=A-Net Game Server
host=game.a-net-online.lol
port=513
client_user=%USER%
server_user=%ALIAS%
term=ansi/115200
expect_ack=1
```

The destination ID is:

```text
ANETGAMES
```

The BBS launches it with:

```text
ANETRLGN.EXE ANETGAMES
```

`ANETRLB.EXE` looks up `ANETGAMES` in `ANETRLGN.INI` and makes the
configured outbound RLogin connection.

---

# 🕹️ Multiple RLogin Destinations

ANetRLogin can provide **multiple RLogin destinations using the same
ANETRLGN.EXE**.

For example:

```ini
[destination ANETGAMES]
name=A-Net Game Server
host=game.a-net-online.lol
port=513
client_user=%USER%
server_user=%ALIAS%
term=ansi/115200
expect_ack=1

[destination MYSTIC]
name=My Mystic BBS
host=mystic.example.com
port=513
client_user=%USER%
server_user=%ALIAS%
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

Then create separate BBS menu entries:

```text
ANETRLGN.EXE ANETGAMES
ANETRLGN.EXE MYSTIC
ANETRLGN.EXE OTHERBBS
```

There is no need to install separate copies of `ANETRLGN.EXE`.

The destination ID determines which configured server is used.

> **Important:** Restart `ANETRLB.EXE` after changing `ANETRLGN.INI`.

---

## 👤 User and Alias Mapping

ANetRLogin can insert information about the current BBS caller into the
RLogin handshake.

Available macros:

```text
%USER%     BBS user name
%ALIAS%    BBS alias / handle
```

For example:

```ini
client_user=%USER%
server_user=%ALIAS%
```

Because RLogin servers can use the two username fields differently, these
values can be configured independently for every destination.

---

# ▶️ Starting the Windows Bridge

Before callers can use ANetRLogin, start the Windows bridge.

From a Windows command prompt:

```bat
CD /D C:\SF\ANETRLGN
ANETRLB.EXE
```

The bridge will report the number of allowlisted destinations loaded from
the configuration.

Leave `ANETRLB.EXE` running while ANetRLogin is available to callers.

---

# 🚪 Spitfire Door Command

For the A-Net Game Server:

```text
C:\SF\ANETRLGN\ANETRLGN.EXE ANETGAMES
```

Additional menu entries simply use another configured destination ID:

```text
ANETRLGN.EXE ANETGAMES
ANETRLGN.EXE MYSTIC
ANETRLGN.EXE OTHERBBS
```

This makes it easy to create an entire **Outbound RLogin / Other BBSes /
Game Servers** menu inside a classic DOS BBS.

---

# 🔙 Disconnecting

While connected through ANetRLogin, press:

```text
ESC ESC ESC
```

three times quickly.

ANetRLogin will disconnect the outbound RLogin session and return the
caller to the BBS.

---

# ⚡ v0.2.0 — Low-Latency Release

Version 0.2.0 introduces major improvements to interactive responsiveness.

The relay engine uses:

- Non-blocking network relay behavior
- `TCP_NODELAY`
- Faster IPC polling
- Immediate outbound keystroke processing
- Immediate draining of available inbound network traffic
- Reduced idle polling delays

These changes significantly improve the feel of interactive ANSI BBS
sessions compared with the original relay implementation.

---

# 🔐 Security

ANetRLogin uses a strict **destination allowlist**.

A caller does **not** supply a hostname or TCP port.

Instead, the BBS launches a predefined destination ID:

```text
ANETRLGN.EXE ANETGAMES
```

The actual hostname, port, RLogin fields, and terminal configuration are
controlled by the sysop in `ANETRLGN.INI`.

This design prevents callers from using ANetRLogin as an unrestricted
outbound TCP proxy.

### RLogin Security Notice

RLogin is a **plaintext protocol**.

ANetRLogin does not add encryption to RLogin traffic.

Use RLogin only with systems you trust and preferably across trusted/private
networks or VPN paths where appropriate.

---

# 🛠️ Troubleshooting

If the door launches but does not connect:

1. Confirm `ANETRLB.EXE` is running.
2. Confirm the destination ID exactly matches a `[destination ID]` in `ANETRLGN.INI`.
3. Confirm the configured hostname and TCP port are correct.
4. Confirm the Windows machine can reach the remote RLogin server.
5. Confirm `BBSDir` points to the directory containing the live `DOOR.SYS`.
6. Confirm `DoorDir` points to the ANetRLogin working directory.
7. Restart `ANETRLB.EXE` after making changes to `ANETRLGN.INI`.

---

# 🏁 Live Tested

ANetRLogin isn't just a proof of concept.

It was developed and tested against a **live Spitfire BBS running under
Windows 7 32-bit**, using its normal DOOR.SYS/FOSSIL environment.

The production path on A-Net Online is:

```text
Caller
  │
  ▼
Spitfire BBS
  │
  ▼
ANETRLGN.EXE
  │
  ▼
ANETRLB.EXE
  │
  ▼
RLogin
  │
  ▼
A-Net Game Server / Remote RLogin Service
```

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

### ANetRLogin

**Bringing modern RLogin connectivity to classic DOS BBS software.**
