# ANetRLogin v0.2.5

- Fixed the Win32 local viewer clipping the right edge of standard 80x25 screens.
- A-Net Game Server remains 80x25; no 132-column mode is required.
- Viewer sizing now uses the largest actual `Terminal` font cell metric reported
  by Windows before `AdjustWindowRectEx()` calculates the outer window size.
- `viewer_cols` / `viewer_rows` remain available for destinations that genuinely
  use other terminal geometries.
- Caller/FOSSIL relay remains unchanged from the known-good v0.2.4 path.
- Local viewer remains read-only and independent of the caller session.
- `%USER%`, `%ALIAS%`, `%PASSWORD%`, 10-node support, 32 destinations, and the
  tested Synchronet password-first/user-second mapping remain unchanged.

# ANetRLogin v0.2.4

- Removed the experimental DOS-side `od_disp_emu()` local-mirror path from
  v0.2.3. The DOS door is back to the known-good caller-only relay using
  `od_disp(..., FALSE)`.
- Added `ANETRLV.EXE`, a separate 32-bit Windows ANSI/CP437 local viewer.
- `ANETRLB.EXE` automatically launches one viewer per active BBS node when
  `[general] local_viewer=1`.
- The bridge mirrors inbound remote terminal bytes to `ARLnn.VWR` for the
  viewer without changing the caller data path.
- Added `ARLnn.VMI` safe viewer metadata. Passwords and expanded RLogin
  credentials are never written to viewer metadata or displayed by the viewer.
- Viewer windows close automatically after a session ends; manually closing a
  viewer does not disconnect the caller.
- Corrected the default/tested Synchronet mapping to password first, user
  second: `client_user=%PASSWORD%`, `server_user=%USER%`.
- Retains `%USER%`, `%ALIAS%`, `%PASSWORD%`, 10-node support, 32 destinations,
  low-latency non-blocking relay, and one shared bridge for all nodes.

