# LAN Realtime Share

A LAN-only realtime sharing app built with Go.

- Realtime text sync across devices
- File and folder upload/download from browser
- Direct Share mode (serve host files/folders directly, with optional one-time code)
- No cloud dependency: works inside your local network

Developer: Dipak Shrestha  
GitHub: https://github.com/DipakShrestha-ADS

---

## 1) Clone the project

```bash
git clone https://github.com/DipakShrestha-ADS/lan-realtime-share.git
cd lan-realtime-share
```

---

## 2) Build binaries ( Ignore this if you are cloning )

### Prerequisite
- Go 1.23+

### Build using Makefile

```bash
make build-linux
make build-linux-arm64
make build-mac
make build-mac-arm64
make build-win-from-linux   # from Linux/macOS for Windows target
# or on Windows host:
make build-win
```

Built files are generated in `output/`.

---

## 3) Run **after build** (step-by-step)

This section is for running the compiled app from `output/`.

### 3.1 Verify `output/` has required files

Expected structure:

```text
output/
├─ lan-realtime-share-linux-amd64
├─ lan-realtime-share-linux-arm64
├─ lan-realtime-share-mac-amd64
├─ lan-realtime-share-mac-arm64
├─ lan-realtime-share.exe
├─ static/
└─ templates/
```

> `uploads/` is created automatically at runtime (in your current working directory).

### 3.2 Choose the correct binary for your OS/architecture

| OS | Architecture | Binary |
|---|---|---|
| Linux | x86_64 / amd64 | `output/lan-realtime-share-linux-amd64` |
| Linux | arm64 | `output/lan-realtime-share-linux-arm64` |
| macOS (Intel) | amd64 | `output/lan-realtime-share-mac-amd64` |
| macOS (Apple Silicon M1/M2/M3) | arm64 | `output/lan-realtime-share-mac-arm64` |
| Windows | amd64 | `output/lan-realtime-share.exe` |

### 3.3 Start the server from inside `output/`

Running from `output/` ensures `static/` and `templates/` are found correctly.

#### macOS / Linux

```bash
cd output
chmod +x ./lan-realtime-share-mac-arm64   # change file name for your platform
./lan-realtime-share-mac-arm64
```

#### Windows (PowerShell)

```powershell
cd output
.\lan-realtime-share.exe
```

Server starts on:
- `http://localhost:9000`
- `http://<YOUR_LAN_IP>:9000`

---

## 4) Open on other devices

1. Connect all devices to the same Wi-Fi/LAN.
2. Start server on host machine.
3. Open in browser:
   - Host: `http://localhost:9000`
   - Other devices: `http://<HOST_LAN_IP>:9000`

Find LAN IP:
- Windows: `ipconfig`
- macOS: `ipconfig getifaddr en0` (or `en1`)
- Linux: `ip a`

---

## 5) How to use the app

## Home page (`/`)

### Realtime Text
- Type in text box on any device.
- Text syncs in realtime to all connected devices.

### File Sharing
- `Upload Files`: select multiple files.
- `Upload Folder`: upload folder with structure preserved.
- Download files directly.
- Download folders as ZIP.
- Upload progress shows percent, speed, ETA.
- Uploaded items auto-expire after ~2 hours.

### Delete / Clear behavior
- Only server-host browser can:
  - delete file/folder
  - clear all uploads
- Client browsers can still view and download.

## Direct Share page (`/direct-share`)

Use this when host wants to share an existing absolute path directly from host disk.

- Add direct share from host browser only.
- Can share file or folder.
- Optional one-time code for secure first download.
- Resume-friendly download support.

### Important recommendation
- Sharing a ZIP file is usually faster and more stable than sharing a raw folder.
- For best performance, compress folder to `.zip` before sharing.

### Direct Share permissions
- Only server-host browser can:
  - add direct share
  - remove one direct share
  - remove all direct shares
- Client browsers can list and download shared entries.

---

## 6) Run from source (alternative)

```bash
go run cmd/server/main.go
```

Then open `http://localhost:9000`.

---

## 7) Docker (optional)

Project includes Docker files, but app listens on port `9000` internally.
If Docker mapping is incorrect in your environment, map host and container to `9000`.

Typical mapping should be:

```yaml
ports:
  - "9000:9000"
```

Then open `http://localhost:9000`.

---

## 8) API endpoints (quick reference)

| Route | Method | Purpose |
|---|---|---|
| `/` | GET | Main page |
| `/direct-share` | GET | Direct Share page |
| `/ws` | GET | WebSocket realtime sync |
| `/upload` | POST | Upload files/folder content |
| `/files` | GET | List uploaded items |
| `/files/{path}` | GET | Download uploaded file |
| `/download-zip?path=...` | GET | Download uploaded folder/file as zip |
| `/delete?path=...&isFolder=true/false` | GET | Delete uploaded item (host only) |
| `/clear-all` | POST | Clear all uploads (host only) |
| `/direct-share/register` | POST | Add direct share (host only) |
| `/direct-shares` | GET | List direct shares |
| `/direct-share/download?id=...` | GET | Download direct share |
| `/direct-share/delete?id=...` | POST | Delete direct share (host only) |
| `/direct-share/clear-all` | POST | Delete all direct shares (host only) |
| `/direct-share/host-info` | GET | Host info + capability flags |

---

## 9) Troubleshooting

- **Server starts but page not loading on other device**
  - Check both devices are on same LAN
  - Allow app/port `9000` in firewall
  - Use `http://` (not `https://`)
- **Styles/UI broken**
  - Ensure you started binary from `output/` where `static/` and `templates/` exist
- **Cannot add/delete/clear from client device**
  - Expected behavior: these actions are host-only by design
- **Path not found in Direct Share**
  - Path must exist on server host machine (absolute path)

---

## 10) Security notes

- This app is intended for trusted LAN environments.
- Anyone on the same LAN can access shared content unless your network is restricted.
- One-time code in Direct Share protects initial access to that share.

---

## 11) License / Attribution

Project by Dipak Shrestha.  
GitHub: https://github.com/DipakShrestha-ADS
